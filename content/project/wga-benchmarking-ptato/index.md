---
title: "Benchmarking Single-Nucleus Whole-Genome Amplification for Low-Frequency Variant Detection"
summary: Evaluated sensitivity/specificity tradeoffs of PTA vs. MDA whole-genome amplification for single-nucleus variant calling using a Nextflow-based workflow (PTATO), to inform assay choice for low-VAF somatic variant analysis.
authors:
- admin
tags:
- Data analysis
- Somatic variant calling
- CNV calling
categories: []
date: "2024-06-01T00:00:00Z"
external_link: ""
image:
  caption: ""
  focal_point: Smart
links: []
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""
slides: ""
---
Detecting low-frequency somatic variants in single nuclei requires whole-genome amplification (WGA), but different WGA chemistries introduce different amplification artifacts and allelic dropout profiles. To choose the right assay for downstream variant-calling work, I benchmarked two single-nucleus WGA modalities - Primary Template-directed Amplification (PTA) and Multiple Displacement Amplification (MDA) - using a unified Nextflow-based workflow (PTATO) to quantify genome-wide allelic dropout and amplification bias for each method.

This analysis quantified the sensitivity/specificity tradeoffs of each modality for low-frequency variant detection, directly informing assay selection for the single-nucleus whole-genome sequencing components of the broader somatic mosaicism project. The same evaluation framework generalizes to any single-cell/nucleus WGA-based variant discovery pipeline where amplification bias needs to be characterized before it affects downstream calls.
