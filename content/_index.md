---
date: "2026-07-06"
sections:
- block: about.biography
  content:
    title: About Me
    username: admin
  id: about
- block: experience
  content:
    title: Experience
    date_format: Jan 2006
    experience:
    - title: Graduate Research Assistant
      company: UNC Chapel Hill \| Heinzen Lab, Eshelman School of Pharmacy
      company_url: 'https://pharmacy.unc.edu/research/faculty-labs/erin-heinzen/'
      location: Chapel Hill, NC
      date_start: '2020-01-01'
      date_end: '2026-05-01'
      description: |2-
          Integrated experimental and computational omics approaches to study how mosaic genotype influences cell state and disease phenotype in human brain tissue.

          - Designed and executed bulk and single-cell sequencing assays (WGS/WES, duplex sequencing, Smart-seq2, WGA/PTA/MDA, SoMoSeq); led downstream CNV/LOH and snRNA-seq analyses from study design through biological interpretation
          - Developed reproducible HPC-based bioinformatics workflows for large-scale RNA-seq and NGS analysis, incorporating batch correction and model diagnostics to preserve biology across heterogeneous samples
          - Partnered with clinical and experimental teams across a 300+ case cohort to integrate genomic findings with clinical phenotypes for variant prioritization and interpretation, improving diagnostic yield in previously unresolved cases
    - title: Computational Biology Intern
      company: Biogen
      location: Boston, MA
      date_start: '2023-06-01'
      date_end: '2023-08-31'
      description: |2-
          - Established standardized cell-type annotation nomenclature and QC thresholds across 4 neurodegenerative disease programs (ALS, AD, PD, MS; 3.5M+ cells), improving cross-disease consistency and reuse by 3+ therapeutic-area teams
          - Integrated large-scale public references (Allen Brain Atlas, CELLxGENE, GTEx) with internal scRNA-seq data to support a comprehensive CNS reference atlas for cross-disease biomarker discovery
    - title: Graduate Research Assistant
      company: Columbia University Medical Center
      location: New York, NY
      date_start: '2018-06-01'
      date_end: '2020-05-01'
      description: |2-
          - Analyzed longitudinal transcriptomic time-series data (n=25 mice) to identify early molecular programs associated with neuromuscular degeneration and aging-related functional decline
          - Evaluated neurotoxic effects of heavy metals and organophosphates in iPSC-derived motor neurons and ALS mouse models using dose-response, immunostaining, and behavioral phenotyping assays
    - title: Graduate Intern, VelociGene
      company: Regeneron Pharmaceuticals
      location: Tarrytown, NY
      date_start: '2019-06-01'
      date_end: '2019-08-31'
      description: |2-
          - Integrated transcriptomic and imaging-derived phenotypic data from CRISPR-edited cellular models in a cisplatin-induced ototoxicity screen, prioritizing candidate genes for translational follow-up
  design:
    columns: "1"
  id: experience
- block: features
  content:
    title: Technical Skills
    items:
    - name: Programming & Statistics
      icon: code
      icon_pack: fas
      description: 'R, Python, Bash, SQL; mixed-effects/hierarchical models, covariate-adjusted association testing, classification & regression (lme4, scikit-learn), variant burden testing'
    - name: Transcriptomics
      icon: chart-line
      icon_pack: fas
      description: 'Bulk & single-cell/nucleus RNA-seq/ATAC-seq; alignment (STAR, kallisto, Cell Ranger), QC, batch correction, clustering & annotation (Seurat, Scanpy, popV), differential expression (limma, edgeR, DESeq2, MAST, dreamlet)'
    - name: Multi-Omics
      icon: layer-group
      icon_pack: fas
      description: 'Genotype-expression linkage, donor/batch-aware pseudobulk analysis, cell-state discovery via variational inference (scVI-tools, scANVI, WGCNA), gene regulatory network inference (SCENIC)'
    - name: Genomic Analysis
      icon: dna
      icon_pack: fas
      description: 'WES/WGS/duplex sequencing processing, somatic/germline variant calling, CNV/LOH/SV identification, variant prioritization (GATK, Mutect2, BWA, MoChA, ANNOVAR)'
    - name: Compute & Reproducibility
      icon: server
      icon_pack: fas
      description: 'HPC/Slurm, containerized environments, workflow orchestration (Nextflow), cloud fundamentals (AWS S3, Batch), Git, Conda, Singularity, R Shiny'
    - name: Experimental / Wet Lab
      icon: flask
      icon_pack: fas
      description: 'Nuclei isolation, Smart-seq2, NGS library prep, WGA/PTA/MDA/PicoPLEX, TaqMan & digital PCR, flow cytometry, tissue dissociation, immunofluorescence/confocal microscopy'
  id: skills
- block: portfolio
  content:
    buttons:
    - name: All
      tag: '*'
    - name: Single-Cell Multi-Omics
      tag: Single cell RNA sequencing
    - name: Variant & Genomic Analysis
      tag: Data analysis
    default_button_index: 0
    filters:
      folders:
      - project
    title: Projects
  design:
    columns: "1"
    flip_alt_rows: false
    view: showcase
  id: projects
- block: collection
  content:
    filters:
      exclude_featured: true
      folders:
      - publication
    title: Publications
  design:
    columns: "2"
    view: publication
  id: publication
- block: collection
  content:
    filters:
      exclude_featured: false
      folders:
      - event
    title: Talks
  design:
    columns: "2"
    view: citation
  id: talks
- block: collection
  content:
    filters:
      exclude_featured: true
      folders:
      - poster
    title: Posters
  design:
    columns: "2"
    view: citation
  id: poster
- block: accomplishments
  content:
    title: Awards & Training
    date_format: Jan 2006
    item:
    - title: Predoctoral Fellowship
      organization: American Epilepsy Society
      date_start: '2024-01-01'
      description: ''
    - title: Single Cell Analysis Course (Regeneron Scholars Program)
      organization: Cold Spring Harbor Laboratory
      date_start: '2022-01-01'
      description: ''
  design:
    columns: "1"
  id: awards
- block: collection
  content:
    count: 5
    filters:
      author: ""
      category: ""
      exclude_featured: false
      exclude_future: false
      exclude_past: false
      folders:
      - post
      publication_type: ""
      tag: ""
    offset: 0
    order: desc
    subtitle: ""
    text: ""
    title: Recent Posts
  design:
    columns: "2"
    view: compact
  id: posts
- block: contact
  content:
    address:
      city: Chapel Hill
      country: United States
      country_code: US
      postcode: ""
      region: NC
      street: ""
    appointment_url: ""
    autolink: true
    contact_links:
    - icon: linkedin
      icon_pack: fab
      link: https://www.linkedin.com/in/meethilagade/
      name: Connect on LinkedIn
    - icon: github
      icon_pack: fab
      link: https://github.com/brain-discourse
      name: See my code
    email: meethilagade@gmail.com
    subtitle: null
    text: "I'm currently completing my PhD and looking for my next role in computational biology / bioinformatics in industry. Feel free to reach out:"
    title: Contact
  design:
    columns: "2"
  id: contact
title: null
type: landing
---
