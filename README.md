# Delivery Timing and Customer Dissatisfaction in E-Commerce
### An Asymmetry Analysis Across Product Categories

Selected analysis code and all result tables for my MSc thesis in Economics and Business (Data Science and Marketing Analytics) at the
Erasmus School of Economics, Erasmus University Rotterdam.

**Author:** Sinem Kırcalı · **Supervisor:** Hans Ligtenberg · **Second assessor:** Nuno Camacho

---

## Summary

Online shoppers know almost everything before paying except when the order will arrive, so the promised
delivery date becomes their benchmark. Drawing on Expectancy Disconfirmation Theory and Prospect Theory,
this thesis analyzes **95,830 fulfilled orders** from the Brazilian marketplace Olist in three phases:

| Phase | Hypothesis | Method | Result |
|---|---|---|---|
| 1 | **H1** Delivery timing is the strongest structured predictor of dissatisfaction, overall and within every product category | Random Forest / XGBoost / Logistic Regression; drop-column ablation and TreeSHAP | **Supported.** Removing the promised-date deviation lowers holdout ROC-AUC from 0.750 to 0.637, about 19× the next-largest loss; it ranks first in all six category groups |
| 2 | **H2a** Lateness is more strongly associated with dissatisfaction than earliness of the same size | Seller-clustered logistic regression with piecewise (hinge) lateness and earliness terms; seller-cluster bootstrap | **Supported.** In the first week, each late day multiplies the odds of a 1–2-star review by about 1.6, while each early day lowers them by about 5% |
| 2 | **H2b** The asymmetry differs in size across product categories | Category × timing interactions; 20-df joint Wald test of the asymmetry function | **Not supported.** The asymmetry is present in every category (all p < 0.001) but does not differ significantly between them (χ²(20) = 19.21, p = 0.509) |
| 3 | **H3** Delivery-related complaints are recurrent and modal across categories | Multi-seed LDA topic modeling of 9,118 dissatisfied written reviews | **Supported.** Late or missing orders are the dominant theme in 64% of complaints and the modal theme in every category |

All results are observational associations, not causal effects.

---

## What is published, and what is not

This is a partial release. The code for Parts 0–2 of the notebook is published with its saved outputs; the code for Parts 3–5 is not.
The result tables and figures of every phase are in `output/`.

| Notebook part | Thesis section | Code |
|---|---|---|
| Part 0 — Setup | – | Published |
| Part 1 — Data and sample construction | §3, §4.1 | Published |
| Part 2 — Machine-learning classification (H1) | §4.2, §5.1 | Published |
| Part 3 — Asymmetry test (H2a) | §4.3, §5.2.1–5.2.3 | Withheld |
| Part 4 — Product-category moderation (H2b) | §4.4, §5.2.4 | Withheld |
| Part 5 — Text analysis (H3) | §4.5, §5.3 | Withheld |

**Why parts of the code are withheld.** Parts 3–5 are the most distinctive part of the work: the asymmetry models, the
category-moderation tests and the text-analysis pipeline. I would rather not have that implementation copied and presented
as someone else's work, so it is not published. The results of every phase are, and the withheld code can be shared on
request, for example for an interview (see the contact section of [my portfolio](https://sinem-kircali.github.io/)).

For this reason the repository shows the work and its results, but it is not a package for replicating the full thesis.

---

## Data

The analysis uses the public [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
(Kaggle). The raw data is **not** included in this repository. Of the review texts used in the text analysis, only a few
representative reviews per topic are included (in `output/`).

---

## Output folder

`output/` contains the result tables (CSV) and figures (PNG) of all three phases. The full set of review texts, the full
vocabulary list and run-level files (every individual topic-model run, pairwise run comparisons and simulation draws) are
not included.

---

## Use

© 2026 Sinem Kırcalı. All rights reserved. The code and tables are published for viewing; please do not reuse or
redistribute them without permission. To refer to this work, please cite it as: Kırcalı, S. (2026). *Delivery timing and
customer dissatisfaction in e-commerce: An asymmetry analysis across product categories* [MSc thesis]. Erasmus University Rotterdam.
