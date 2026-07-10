---
title: "CNS Single-Nucleus Reference Atlas for Cell-Type Annotation"
summary: Built a unified cortical reference atlas from published single-nucleus RNA-seq datasets to evaluate cell-type annotation robustness in human brain single-nucleus multi-omic data.
authors:
- admin
tags:
- Single cell RNA sequencing
- Data analysis
- Multi-omics
categories: []
date: "2025-05-01T00:00:00Z"
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
bibliography: references.bib
---
As part of my doctoral work, I built a reference label transfer framework to evaluate the robustness of cell-type annotation in human cortical single-nucleus RNA-seq data. The goal was not to replace primary marker-based annotations, but to use independent reference-derived labels as a diagnostic comparison for annotation confidence and downstream genotype-resolved transcriptomic interpretation. I constructed a unified, region-matched cortical reference atlas from publicly available transcript-end and full-length single-nucleus RNA-seq datasets, including Siletti et al.,[@siletti2023] Zhu et al.,[@zhu2023] Hodge et al.,[@hodge2019] and Tasic et al.[@tasic2018] Only cortical samples from donors under 30 years old were retained to support consistent comparison across studies. Reference datasets were processed using standardized QC thresholds and normalization, then integrated with Harmony.[@korsunsky2019] The resulting atlas used consistent cell-type nomenclature and provided an independent sensitivity check for cell-type identity, annotation robustness, and interpretation of genotype-resolved expression patterns in mosaic human brain tissue.