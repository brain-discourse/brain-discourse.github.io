# latch-variant: A Human-in-the-Loop Agentic Framework for Curating and Interpreting Genomic Variant Data

**Design proposal drafted by Meethila Gade in preparation for a Latch Bio technical interview, July 2026.** *Modeled directly on Kenny Workman's latch-curate whitepaper (June 27, 2025, latch.bio/latch-curate). Not an official Latch Bio document — a working exercise in applying their published curation-engineering philosophy to a new domain.*

------------------------------------------------------------------------

## Abstract

latch-curate showed that single-cell transcriptional curation — count-matrix construction, cell typing, metadata harmonization — is well suited to human-in-the-loop agentic workflows because the task decomposes into concrete steps, each with testable, formally verifiable outputs. Genomic variant curation shares this exact structure, but with a distinct and important complication: the outputs are not research-atlas annotations, they are clinical- and population-genetic claims that can directly change a diagnosis, a risk assessment, or a treatment decision. We propose **latch-variant**, an agentic framework that guides an expert curator through an ordered lifecycle — callset harmonization, quality control, annotation, variant classification, and cohort/phenotype metadata harmonization — using the same five LLM engineering principles latch-curate established, while introducing additional guardrails specific to the higher stakes and faster-churning reference knowledge base of variant interpretation.

------------------------------------------------------------------------

## 1. Introduction

Aggregated public variant data — ClinVar, gnomAD, dbSNP, cohort-level VCFs deposited alongside GWAS and disease-sequencing studies — forms the largest and most diverse repository of human genetic variation in existence, spanning far more ancestries, diseases, and rare presentations than any single institution could sequence itself. Yet, as with public scRNA-seq data before curation tools existed, this resource is inconsistently structured and non-uniformly interpreted: submitting labs use different reference builds, different normalization conventions for the same indel, different ACMG evidence-weighting judgment calls for the same variant, and different levels of rigor in linking a variant call back to a phenotype. The result, as documented across the ClinVar literature, is that a meaningful fraction of variants carry **conflicting classifications between submitters** — the exact same "quality erosion from human variability" problem latch-curate identified in single-cell metadata, but with direct clinical consequence instead of research-annotation noise.

The curation lifecycle for variants decomposes into concrete, testable steps in the same way single-cell curation does: build a harmonized callset from heterogeneous raw inputs, apply quality control, annotate consequence and population frequency, assign a controlled classification, and harmonize the cohort- and phenotype-level metadata needed to interpret the variant in context. Each step produces an artifact a human expert can inspect, correct, and approve before the pipeline advances — the same lifecycle structure, applied to a domain where an unreviewed agent mistake is not a mislabeled cluster in a research atlas, but a wrong answer in a variant report.

We hypothesize this problem is equally well suited to agentic workflows as single-cell curation, provided the framework is built around the same engineering discipline latch-curate established — plus additional constraints specific to variant interpretation's clinical stakes and its unusually fast-moving reference knowledge base (ClinVar reclassifies existing variants continuously; reference builds and gene-disease validity assertions get revised on a rolling basis, unlike a static cell-type ontology).

------------------------------------------------------------------------

## 2. Methods

### 2.1 System Design

#### 2.1.1 LLM Engineering Principles (adopted directly from latch-curate)

1.  **End-to-End Reasoning.** As in latch-curate, task context, control-flow decisions, and tool selection are embedded in a single model call per step rather than split across rigid sub-agents — variant interpretation, like curation, benefits from a model reasoning over the whole local context (paper text, cohort metadata, annotation output) at once rather than through a brittle fixed pipeline of specialist agents.

2.  **Precise Validation Criteria.** Every step pairs a natural-language description with a code assertion. For variant curation, criteria include things like: *all variant IDs are normalized (left-aligned, parsimonious representation)*, *every classified variant cites at least one applied ACMG/AMP evidence code*, *reported genome build is consistent across all annotation sources used*, *sample-level sex inferred from the data matches sex recorded in metadata (or the discrepancy is explicitly flagged, not silently dropped)*.

3.  **Domain Knowledge as Prompts and Tools.** Reusable tools: a variant-normalization library (guaranteeing consistent representation before any merge or comparison — directly the hap.py/vcfeval normalization problem), an ACMG/AMP evidence-code checklist tool, ontology-search tools for HPO/MONDO/ClinGen gene-disease validity lookups, and a liftover-consistency checker. Prompts accumulate edge cases the same way latch-curate's did — e.g., how to handle a variant that is pathogenic in one transcript but synonymous in another, or a locus with a known pseudogene/segmental-duplication mapping confound (directly the GATK/Strelka2 homopolymer-and-segdup failure modes from this candidate's own take-home).

4.  **Output Integration.** The model writes classification decisions and evidence trails to fixed JSON schemas (variant ID, applied ACMG codes, cited evidence sources, confidence tier, model reasoning trace) validated in code, with automatic retries on schema failure — identical mechanism to latch-curate's AnnData-schema validation.

5.  **Chain-of-Thought Traces.** Required and surfaced to the curator verbatim, exactly as in latch-curate — arguably more important here, since a curator reviewing a pathogenicity call needs to see *which* evidence lines the model weighted and why, not just the final tier, in order to sign off responsibly.

#### 2.1.2 Variant Curation Principles

1.  **Understanding the Assignment.** Mirrors latch-curate's manual-curation-first approach: before automating, build deep task understanding by manually curating a meaningful volume of variants across a defined set of conditions (e.g., several thousand variants across \~50 disease-gene panels spanning both germline and clinically-actionable somatic contexts) to learn which parts of interpretation are conserved (population-frequency filtering, computational-predictor triage) versus which truly require case-by-case judgment (segregation evidence, functional-assay interpretation, conflicting ClinVar submissions).

2.  **Ontology-Driven Variables — with one important difference from latch-curate.** Single-cell curation had to *choose* a granularity level for an immature, actively-debated ontology (Cell Ontology "level 1"). Variant classification is comparatively more mature here: ACMG/AMP's five-tier system (Pathogenic / Likely Pathogenic / VUS / Likely Benign / Benign) is already the field's controlled vocabulary, and ClinGen gene-disease validity classifications are similarly standardized — so latch-variant doesn't need to invent a granularity scheme the way latch-curate did for cell types. What is *not* mature or standardized is the **evidence assembly** behind reaching that tier — the 28 ACMG evidence codes combine population frequency, computational prediction, segregation, de novo status, and functional data in ways that are far more heterogeneous and multi-source than a differential-expression-to-cell-type mapping. The hard ontology-engineering problem in latch-variant is therefore evidence-code assembly and citation, not label-set design.

3.  **Validation Artifacts.** Before/after reports analogous to latch-curate's violin plots, but variant-specific: allele-balance and depth distributions before/after QC (directly this candidate's own take-home Step 5 methodology), a precision-recall summary against a truth set for any newly incorporated caller or pipeline version, and — for classification specifically — a structured evidence table showing exactly which ACMG codes were applied and from which source (gnomAD frequency, REVEL/CADD/AlphaMissense score, ClinVar precedent, segregation data) so a curator can audit the call in under a minute rather than re-deriving it from scratch.

4.  **Parallel Agentic Workflows.** Same throughput principle as latch-curate — a single classification task might take minutes of agent time before needing human review, so curator throughput scales with how many concurrent runs keep the review queue full, particularly valuable for clearing ClinVar's backlog of conflicting/unclassified submissions at scale.

5.  **Provenance and Audit Trail as a first-class requirement (new relative to latch-curate).** Because outputs here can influence clinical management, every classification needs a complete, immutable audit trail — which model version, which evidence sources, which curator approved it, and when — sufficient to support CAP/CLIA-style analytical validation and to support re-review if a gene-disease validity assertion or population frequency reference later changes. latch-curate's version-control tooling (versioned containers, blob-stored driver scripts and logs) is necessary but not sufficient here; variant curation additionally needs the audit trail to be reviewable by someone who was not the original curator, since clinical sign-off standards generally require independent second review.

### 2.2 Curation Workflow

Six concrete tasks, mapped directly onto latch-curate's six-step structure:

#### 2.2.1 Data Ingestion (no LLM, same as latch-curate)

Aggregate paper text, cohort/study metadata, and raw VCF/BAM or summary-statistic files from public repositories (SRA, dbGaP, EGA, ClinVar submission records, GWAS Catalog). As in latch-curate, minimal upfront structuring — later agentic steps reason over the raw HTML/tabular/text directly.

#### 2.2.2 Callset Harmonization (agentic — the analog of latch-curate's count-matrix construction, and likely the most time-consuming step for the same reason)

The model is placed in a sandboxed environment with terminal access and instructed to write code that normalizes and merges heterogeneous input callsets — different reference builds, different callers, different variant representations for the same underlying indel — into a single well-defined multi-sample VCF, iterating against validation tests until they pass. Example validation criteria, mirroring latch-curate's exact style: - all variant records are left-aligned and parsimoniously represented (no redundant padding bases), - every record's REF allele matches the stated reference build at that position, - sample IDs are unique and consistently named across all merged sources.

This step directly reuses this candidate's own hap.py/vcfeval normalization expertise — variant-representation ambiguity (the same indel written two different ways in two source VCFs) is exactly the problem vcfeval's haplotype-aware comparison solves, and is the single hardest sub-problem in this step, just as unstructured supplement parsing was the hardest sub-problem in count-matrix construction.

#### 2.2.3 Quality Control (agentic, adaptive thresholds — same structure as latch-curate)

Detect the sequencing/calling technology from unstructured metadata, map to first-pass conservative thresholds from a trusted table (depth, genotype quality, Ti/Tv ratio, contamination estimate), then compute per-sample quantile tables and feed them back to the model to set adaptive, per-sample filters. Per-sample and aggregate before/after plots (depth distributions, allele-balance histograms — this candidate's own take-home Step 5 methodology, generalized into a reusable agentic step) go into the validation report for curator review; thresholds are editable JSON, exactly as in latch-curate.

#### 2.2.4 Annotation and Normalization (no LLM, same as latch-curate's count transformation)

Deterministic pipeline: liftover if needed, VEP/ANNOVAR/SnpEff consequence annotation, gnomAD frequency annotation, ClinVar/ClinGen cross-reference. Configurable via JSON, no model involvement — this is the step where reproducibility matters most, since annotation sources update on their own release cadence independent of the curation pipeline.

#### 2.2.5 Variant Classification (agentic — the analog of cell typing)

The model assembles applicable ACMG/AMP evidence codes from the annotated variant (population frequency thresholds, computational predictor scores, ClinVar precedent, gene-disease validity from ClinGen) combined with any available segregation/functional/de novo information from the source paper or supplement, and proposes a classification tier with a structured, cited evidence trail and full chain-of-thought reasoning. Curators can adjust the weighting of any single evidence code or override the tier directly in the output JSON and trigger a rerun — same interaction pattern as latch-curate's cell-typing report, but the report itself needs to expose the evidence table (see 2.1.2 principle 3) rather than only a plot.

#### 2.2.6 Cohort and Phenotype Metadata Harmonization (agentic, same structure as latch-curate's metadata harmonization)

Map sample- and cohort-level metadata (phenotype via HPO, disease via MONDO, ancestry/population labels, sequencing platform, treatment/outcome where relevant) to controlled ontology terms using the same ontology-search-tool pattern latch-curate uses for cell type and tissue.

### 2.3 Tooling

-   **Linting and conversion.** A validation pass over every task's output (consistent variant representation, controlled term sets for classification tiers and phenotype terms), plus VCF-build/format interconversion tooling analogous to latch-curate's AnnData-to-Seurat converter.
-   **Version control and reproducibility, extended for a faster-churning reference layer.** Unlike a cell-type ontology, which is comparatively stable, ClinVar submissions, gnomAD frequency data, and ClinGen gene-disease validity assertions update continuously. latch-variant needs the same versioned-container-plus-mounted-input approach latch-curate uses, but with scheduled re-runs of the classification step specifically whenever an underlying reference source changes materially — the variant-curation analog of NCBench's "continuous benchmark, not a one-time snapshot" design philosophy.
-   **A data portal**, directly analogous to latch-curate's, indexing curated and classified variants with full evidence provenance, searchable and filterable by gene, phenotype, ancestry, or classification tier, and able to deliver harmonized variant sets to internal teams or external partners exactly as latch-curate's portal does for single-cell atlases.

------------------------------------------------------------------------

## 3. Discussion

The core argument transfers cleanly: public variant data, like public scRNA-seq data, is the largest and most diverse available resource, underused because structuring and interpreting it well requires scarce, expensive expert time, and degraded in quality by variability between human curators. Agentic, human-in-the-loop workflows built on the same five engineering principles latch-curate established — end-to-end reasoning, precise validation criteria, domain knowledge encoded as prompts and tools, structured output integration, and mandatory chain-of-thought traces — transfer directly to this domain.

What doesn't transfer without modification is the stakes and the reference-data churn rate. A miscurated cell-type label degrades a research atlas; a miscalled ACMG tier can change a clinical decision. That difference argues for building the audit-trail and independent-second-review requirement into latch-variant as a first-class design principle from the start, rather than as tooling added later — and for treating "the classification pipeline must be cheaply re-runnable whenever ClinVar, gnomAD, or ClinGen updates" as a core requirement rather than a nice-to-have, since unlike a cell ontology release, these reference sources update on the order of weeks, not years.

This proposal is a direct extension of published Latch Bio engineering philosophy into the variant-interpretation domain that the VariantBench take-home, and this role, are built around — the natural next member of the latch-curate / benchmarks.bio family alongside scBench, EpiBench, SpatialBench, and TxBench-PP.