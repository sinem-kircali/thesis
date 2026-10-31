# Delivery Timing and Customer Dissatisfaction in E-Commerce
### An Asymmetry Analysis Across Product Categories

Analysis code for my MSc thesis in Economics and Business (Data Science and Marketing Analytics) at the
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
---

## Repository structure

```
thesis_final_code.ipynb   Complete analysis (data → H1 → H2a → H2b → H3), with code, explanations and all outputs
output/                   Every result table (CSV) and figure (PNG) reported in the thesis
requirements.txt          Exact package versions used for the thesis results
```

The notebook follows the structure of the thesis. Every part starts with a short explanation that names
the thesis section, table or appendix it produces.

| Notebook part | Thesis section | Tables / figures |
|---|---|---|
| Part 0 — Setup | – | – |
| Part 1 — Data and sample construction | §3, §4.1 | Tables 1–2; Appendix J.1, J.7, K |
| Part 2 — Machine-learning classification (H1) | §4.2, §5.1 | Tables 3–6, Figure 1; Appendix A, C, G |
| Part 3 — Asymmetry test (H2a) | §4.3, §5.2.1–5.2.3 | Tables 7–8; Appendix B, D, H.1–H.3, H.5, H.6, J.2 |
| Part 4 — Product-category moderation (H2b) | §4.4, §5.2.4 | Table 9; Appendix H.4, J.3 |
| Part 5 — Text analysis (H3) | §4.5, §5.3 | Tables 10–11, Figure 2; Appendix E, F, I, J.4–J.6 |

The notebook is saved with its outputs, so every result can be read on GitHub without running anything.
All outputs, and every file in `output/`, come from one top-to-bottom run of this notebook. That run was
checked against the thesis: every reported number in the main text and the appendices is reproduced.

---

## Data

The analysis uses the public [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
(Kaggle). The raw data is **not** included in this repository. Download it and place these eight files
in the same folder as the notebook:

```
olist_orders_dataset.csv            olist_products_dataset.csv
olist_order_items_dataset.csv       olist_sellers_dataset.csv
olist_order_payments_dataset.csv    olist_customers_dataset.csv
olist_order_reviews_dataset.csv     product_category_name_translation.csv
```

The notebook writes two intermediate files next to the raw data (`orders_merged.csv`,
`orders_features.csv`) and all results to `output/`.

---

## How to run

```bash
python3.9 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook thesis_final_code.ipynb    # then: Kernel → Restart & Run All
```

- **Seeds:** `RANDOM_STATE = 42` for all sampling and models. The 50 LDA seeds per topic count are derived
  from the master seed `2026`, so no seed is chosen by hand.
- **Runtime:** a full run takes about 10.5 hours on an 8-core Apple M2. The slowest steps are the H1
  hyperparameter grids and drop-column refits, the 2,000-replication seller-cluster bootstrap (H2a) and
  the 550 LDA fits of the topic-count search (H3).
- **Independent parts:** Parts 2–4 do not depend on Part 5. Part 5 (H3) only needs Parts 0–1.

## Reproducibility

The thesis results were produced with **Python 3.9.6 and the exact versions in `requirements.txt`**.
Use these versions to reproduce the numbers. In Phase 1, Random Forest and XGBoost are almost tied
(training-CV ROC-AUC 0.737385 vs 0.737368), so newer library versions can change which model is selected
as the explanation model and, with it, the downstream H1 tables. Phases 2 and 3 use fixed seeds and
deterministic estimators and are stable across runs.
