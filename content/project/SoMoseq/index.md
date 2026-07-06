---
title: "SoMoSeq: Genotype-Informed Single-Nucleus RNA Sequencing of Mosaic Brain Tissue"
summary: A multi-omic single-nucleus method I developed to link somatic variant status directly to transcriptional state in the same nucleus, applied to mosaic brain tissue in hemimegalencephaly and focal cortical dysplasia.
authors:
- admin
tags:
- Single cell RNA sequencing
- Multi-omics
- Method development
- Somatic mutations
categories: []
date: "2023-01-27T00:00:00Z"
external_link: ""
image:
  caption: ""
  focal_point: Smart
links:
- icon: microphone
  icon_pack: fas
  name: ASHG 2024 Talk
  url: /talk/ashg-2024-loh-hmeg/
- icon: microphone
  icon_pack: fas
  name: ASHG 2023 Talk
  url: /talk/ashg-2023-somoseq/
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""
slides: ""
---
Post-zygotically acquired somatic variants contribute to a range of neurological disorders. Mosaic brain tissue from individuals harboring these variants offers a unique opportunity to study cell-type involvement and the cell-type-specific effects of pathogenic variants on transcription, but simultaneously assessing DNA and RNA from the same single cells is technically challenging. I developed **SoMoSeq (Somatic Mosaicism Sequencing)** to address this: a modified version of the G&Tseq protocol that uses biotinylated-oligo(dT)-conjugated streptavidin magnetic beads to physically separate DNA and RNA from single nuclei. The resulting DNA is genotyped with a custom TaqMan assay while the RNA is used to generate full-length cDNA via Smart-seq2.

I benchmarked the approach using mosaic brain tissue harboring known pathogenic somatic variants - PIK3CA (E545K, VAF≈28%) and SLC35A2 (Y145X, VAF≈10%) - alongside age-matched autopsy/stroke controls. After fluorescence-activated nuclear sorting on the neuronal marker NeuN, SoMoSeq genotyped and generated cDNA from 3,072 nuclei; genotyping accurately reflected the variant allele frequencies observed in bulk tissue across neuronal (NeuN+) and non-neuronal (NeuN-) populations, and full-length RNA-seq from single nuclei averaged ~10,000 genes/nucleus with low (~3%) mitochondrial read fractions.

Differential expression analysis revealed significant mTOR pathway upregulation in the PIK3CA sample relative to controls, and an enrichment of the SLC35A2 variant in non-neuronal (NeuN-) cell types in the mosaic tissue - findings consistent with the literature and validating that SoMoSeq resolves cell-type-specific transcriptional consequences of somatic mutations. Follow-on work extended this to a genome-wide single-cell genotyping pilot, which identified a population of cells homozygous for the PIK3CA variant; orthogonal validation with single-cell whole-genome amplification, SNP genotyping arrays, and single-cell whole-genome sequencing traced this to a somatic copy-neutral loss-of-heterozygosity (CN-LOH) event on chromosome 3q, linking a second, LOH-driven mechanism to mTOR hyperactivation in hemimegalencephaly.
