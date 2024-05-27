---
title: "Engineering indel and substitution variants of diverse and ancient enzymes using Graphical Representation of Ancestral Sequence Predictions (GRASP)"
authors:
- Gabriel Foley 
- Ariane Mora 
- Connie M Ross 
- Scott Bottoms 
- Leander Sutzl 
- Marnie L Lamprecht 
- Julian Zaugg 
- Alexandra Essebier 
- Brad Balderson 
- Rhys Newell 
- Raine ES Thomson 
- Bostjan Kobe 
- Ross T Barnard 
- Luke Guddat 
- Gerhard Schenk 
- Joerg Carsten 
- Yosephine Gumulya 
- Burkhard Rost 
- Dietmar Haltrich 
- Volker Sieber 
- Elizabeth MJ Gillam 
- Mikael Boden

date: "2022"
doi: ""

# Schedule page publish date (NOT publication's date).
publishDate: "2017-01-01T00:00:00Z"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["2"]

# Publication name and optional abbreviated publication name.
publication: "PLOS Computational Biology"
publication_short: "PLOS Comput Biol"

abstract: Ancestral sequence reconstruction is a technique that is gaining widespread use in molecular evolution studies and protein engineering. Accurate reconstruction requires the ability to handle appropriately large numbers of sequences, as well as insertion and deletion (indel) events, but available approaches exhibit limitations. To address these limitations, we developed Graphical Representation of Ancestral Sequence Predictions (GRASP), which efficiently implements maximum likelihood methods to enable the inference of ancestors of families with more than 10,000 members. GRASP implements partial order graphs (POGs) to represent and infer insertion and deletion events across ancestors, enabling the identification of building blocks for protein engineering.

To validate the capacity to engineer novel proteins from realistic data, we predicted ancestor sequences across three distinct enzyme families: glucose-methanol-choline (GMC) oxidoreductases, cytochromes P450, and dihydroxy/sugar acid dehydratases (DHAD). All tested ancestors demonstrated enzymatic activity. Our study demonstrates the ability of GRASP (1) to support large data sets over 10,000 sequences and (2) to employ insertions and deletions to identify building blocks for engineering biologically active ancestors, by exploring variation over evolutionary time.

# Summary. An optional shortened abstract.
summary: Massive sequencing projects expose the extent of natural, genetic diversity. Here, we describe a method with capacity to perform ancestor sequence reconstruction from data sets in excess of 10,000 sequences, poised to recover ancestral diversity, including the evolutionary events that determine present-time biological function and structure.

  We introduce a novel strategy for suggesting “insertion-deletion variants” that are distinct from, but can be explored alongside, substitution variants for creating ancestral libraries. We demonstrate how insertions and deletions can be used as building blocks to form “hybrid ancestors”; based on this strategy, we synthesise ancestor variants, with varying enzymatic activities, for wide-ranging applications in the biotechnology sector. 

tags:
- Inference
featured: true

links:
- name: DOI
  url: https://doi.org/10.1371/journal.pcbi.1010633
# url_pdf: http://arxiv.org/pdf/1512.04133v1
# url_code: '#'
# url_dataset: '#'
# url_poster: '#'
# url_project: ''
# url_slides: ''
# url_source: '#'
# url_video: '#'

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
image:
  focal_point: ""
  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects:
# - internal-project

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides: example
---

