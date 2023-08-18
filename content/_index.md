---
date: "2022-10-24"
sections:
- block: about.biography
  content:
    title: About Me
    username: admin
  id: about
- block: features
  content:
    items:
    - description: 
      icon: r-project
      icon_pack: fab
      name: R
    - description: 
      icon: python
      icon_pack: fab
      name: Python
    - description: 
      icon: dna
      icon_pack: fas
      name: Molecular Biology
    title: Skills
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
- block: portfolio
  content:
    buttons:
    - name: All
      tag: '*'
    - name: Molecular Biology 
      tag: Single cell RNA sequencing
    - name: Computational Biology
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
- block: markdown
  content:
    subtitle: ""
    text: '{{< gallery album="demo" >}}'
    title: Gallery
  design:
    columns: "1"
- block: collection
  content:
    filters:
      featured_only: true
      folders:
      - publication
    title: Featured Publications
  design:
    columns: "2"
    view: card
  id: featured
- block: collection
  content:
    filters:
      exclude_featured: true
      folders:
      - publication
    title: Recent Publications
  design:
    columns: "2"
    view: citation
- block: collection
  content:
    filters:
      folders:
      - Past events
    title: Recent and upcoming talks
  design:
    columns: "2"
    view: compact
  id: talks
- block: tag_cloud
  content:
    title: Popular Topics
  design:
    columns: "2"
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
    - icon: twitter
      icon_pack: fab
      link: https://twitter.com/Twitter
      name: DM Me
    - icon: video
      icon_pack: fas
      link: https://zoom.com
      name: Zoom Me
    email: meethilagade@gmail.com
    phone: 917-673-9459
    subtitle: null
    text: "Feel free to connect with me at:" 
    title: Contact
  design:
    columns: "2"
  id: contact
title: null
type: landing
---
