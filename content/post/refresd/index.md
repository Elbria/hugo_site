---
title: 'Rationalized English-French Semantic Divergences: annotation workflow'
subtitle: 'Bits and pieces missing from (my) paper descriptions.'
summary: bits and pieces missing from (my) paper descriptions.
authors:
- admin
date: "2020-10-27T00:00:00Z"
lastmod: "2020-10-27T00:00:00Z"
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Placement options: 1 = Full column width, 2 = Out-set, 3 = Screen-width
# Focal point options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
image:
  placement: 2
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)'
  focal_point: Center
  preview_only: true

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

In this blog post, I go through (all) annotation decisions involved in the collection of the Rationalized English-French
Semantic Divergences corpus, dubbed [REFreSD](https://github.com/Elbria/xling-SemDiv/tree/master/REFreSD). 
My main goal is **not** to describe REFreSD per se but rather to use it as 
an example to provide a holistic view of the annotation workflow that is
often missing from paper descriptions. Therefore, I will not
cover details that are too specific to the dataset at hand. 
If you are interested in that, you can check out the Appendix of [our paper](https://arxiv.org/pdf/2010.03662.pdf).


This blog provides:  

&nbsp; &nbsp; :heavy_check_mark: a complete depiction of the **annotation workflow**; <br>
&nbsp; &nbsp; :heavy_check_mark: a full description of **Data Statements**; <br>
&nbsp; &nbsp; :heavy_check_mark: a **DataSheet** for REFreSD; <br>
&nbsp; &nbsp; :heavy_check_mark: a list of documents reviewed by the Institutional Review Board at UMD. <br>
 

If you don't know what [Data Statements](https://www.aclweb.org/anthology/Q18-1041.pdf) and 
[Datasheets for Datasets](https://arxiv.org/pdf/1803.09010.pdf) are, follow the links and check out the papers!
 
-------------------------------------------------------------------

### Introduction

REFreSD is published at EMNLP 2020 by  [Eleftheria](http://localhost:1313/#about) and [Marine](https://www.cs.umd.edu/~marine/). 
 
**Motivation:** The project under which REFreSD is collected
 aims to advance our fundamental understanding of the computational representations and methods to compare and contrast text meaning **across languages**. Currently, much cross-lingual work in Natural
 Language Processing relies on the assumption that sentences drawn from parallel corpora are equivalent in meaning. 
 Yet, content conveyed in two distinct languages is rarely exactly equivalent. The ability of 
computational methods to detect such meaning mismatches can be assessed by comparing their predictions with human judgments in
 REFreSD.
  
**Annotation task:** Human annotators were asked to read text excerpts in two languages (e.g., one in English and another in French). We collect their assessment of the meaning differences they observe via sentence-level divergence judgments token-level rationales. 

-------------------------------------------------------------------

### Annotation workflow 

The following chart presents the annotation workflow used for collecting REFreSD. People involved
in each step of the process are listed on the right, while documents those people are presented 
with are shown on the left. 

<img src="files/REFreSD.pdf" height="50px" width="400px" >

-------------------------------------------------------------------


### Documentation

We publish all documents related to the (left side) annotation workflow:

&nbsp; &nbsp; &nbsp; :one: [GUIDELINES](files/REFreSD_Annotation_Guidelines.pdf) includes the exact wording presented to participants; <br>
&nbsp; &nbsp; &nbsp; :two: [PROCEDURES](files/REFreSD_Ethical_Review.pdf) covers text reviewed by the Institutional Review Board at UMD; <br>
&nbsp; &nbsp; &nbsp; :three: [ADVERTISING](files/REFreSD_Recruiting_email.pdf) presents the email used to recruit participants; <br>
&nbsp; &nbsp; &nbsp; :four: [DATA STATEMETS for REFreSD](files/REFreSD_Data_Statements.pdf); <br>
&nbsp; &nbsp; &nbsp; :five: [DATASHEET for REFreSD](files/REFreSD_Datasheet.pdf).<br>

-------------------------------------------------------------------

### Baselines

The Divergent mBERT paper presents several baselines for the prediction of semantic divergences at a sentence and token level.
 Code is available [here](https://github.com/Elbria/xling-SemDiv).

-------------------------------------------------------------------


### Acknowledgments

This dataset was collected after discussions and feedback among the following folks: 
Sweta Agrawal, 
Dennis Asamoah Owusu, 
Valerio Basile,
Emily Bender, 
Tommaso Caselli,
Pranav Goel, 
Ching-Lin Huang, 
Nina Kamooei, 
Marianna Martindale,
Alexander Miserlis Hoyle,
Aquia Richburgh, and
Weijia Xu. Thanks all! 

-------------------------------------------------------------------

### References

* [Detecting Fine-Grained Cross-Lingual Semantic Divergences without Supervision by Learning to Rank](https://arxiv.org/pdf/2010.03662.pdf)
* [Data Statements for Natural Language Processing: Toward Mitigating System Bias and Enabling Better Science](https://www.aclweb.org/anthology/Q18-1041.pdf)
* [Datasheets for Datasets](https://arxiv.org/pdf/1803.09010.pdf) 
