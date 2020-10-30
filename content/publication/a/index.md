---
title: "Detecting Fine-Grained Cross-Lingual Semantic Divergences without Supervision by Learning to Rank"
authors: 
- admin
- Marine Carpuat
date: "2019-08-01"
doi: ""

# Schedule page publish date (NOT publication's date).
#publishDate: "2019-08-01"

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = System Description
publication_types: ["1"]

# Publication name and optional abbreviated publication name.
publication: "Proceedings of EMNLP 2020"
publication_short: "In *EMNLP 2020*"

abstract: Detecting fine-grained differences in content conveyed in different languages matters for cross-lingual NLP and multilingual corpora analysis, but it is a challenging machine learning problem since annotation is expensive and hard to scale. This work improves the prediction and annotation of fine-grained semantic divergences. We introduce a training strategy for multilingual BERT models by learning to rank synthetic divergent examples of varying granularity. We evaluate our models on the Rationalized English-French Semantic Divergences, a new dataset released with this work, consisting of English-French sentence-pairs annotated with semantic divergence classes and token-level rationales. Learning to rank helps detect fine-grained sentence-level divergences more accurately than a strong sentence-level similarity model, while token-level predictions have the potential of further distinguishing between coarse and fine-grained divergences.

# Summary. An optional shortened abstract.
#summary: We show how fine-grained semantic distinctions improve annotation and prediction of cross-lingual semantic divergences.
summary: EMNLP 2020

links:
- icon: file-alt
  icon_pack: fas
  name: pdf
  url: https://arxiv.org/pdf/2010.03662.pdf
- icon: code
  icon_pack: fas
  name: code
  url: https://github.com/Elbria/xling-SemDiv
- icon: database
  icon_pack: fas
  name: data
  url: https://github.com/Elbria/xling-SemDiv/tree/master/REFreSD
- icon: chalkboard
  icon_pack: fas
  name: slides
  url: files/emnlp_2020_presentation.pdf
- icon: chalkboard-teacher
  icon_pack: fas
  name: talk
  url:   
- icon: satellite-dish
  icon_pack: fas
  name: cite
  url: https://arxiv.org/abs/2010.03662


# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
#image:
#  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/s9CC2SKySJM)'
#  focal_point: ""
#  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
#projects: []

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
#slides: "a"
---
