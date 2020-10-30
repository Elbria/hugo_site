---
title: "Cross-Topic Distributional Semantic Representations via Unsupervised Mappings"
authors:
- admin
- Nikos Athanasiou
- Alexandros Potamianos
date: "2019-06-03"
doi: ""

# Schedule page publish date (NOT publication's date).
publishDate: "2019-06-03"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["1"]

# Publication name and optional abbreviated publication name.
publication: "In Proceedings of the 2019 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies, Volume 1 (Long and Short Papers)"
publication_short: "In *NAACL 2019*"

abstract: In traditional Distributional Semantic Models (DSMs) the multiple senses of a polysemous word are conflated into a single vector space representation. In this work, we propose a DSM that learns multiple distributional representations of a word based on different topics. First, a separate DSM is trained for each topic and then each of the topic-based DSMs is aligned to a common vector space. Our unsupervised mapping approach is motivated by the hypothesis that words preserving their relative distances in different topic semantic sub-spaces constitute robust semantic anchors that define the mappings between them. Aligned cross-topic representations achieve state-of-the-art results for the task of contextual word similarity. Furthermore, evaluation on NLP downstream tasks shows that multiple topic-based embeddings outperform single-prototype models.

# Summary. An optional shortened abstract.
summary: NAACL 2019

#tags:
#- Source Themes
#featured: false

links:
- icon: file-alt
  icon_pack: fas
  name: pdf
  url: https://www.aclweb.org/anthology/N19-1110.pdf
- icon: code
  icon_pack: fas
  name: code
  url: https://github.com/Elbria/utdsm_naacl2018
- icon: chalkboard
  icon_pack: fas
  name: slides
  url: files/utdsm_2019.pdf
- icon: chalkboard-teacher
  icon_pack: fas
  name: talk
  url:  https://vimeo.com/355781410
- icon: satellite-dish
  icon_pack: fas
  name: cite
  url: https://www.aclweb.org/anthology/N19-1110.bib



# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/pLCdAaMFLTE)'
  focal_point: ""
  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects: []

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides: ""
---


