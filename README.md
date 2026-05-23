# Predicting On-Time Shipping Delivery

> An e-commerce electronics retailer with its own delivery fleet was losing customers to late deliveries. We built a classifier to predict whether a package would arrive on time — and used the model's feature importance to surface the root cause: a poorly-calibrated discount program overwhelming logistics. Projected impact after deployment: **+23.27% on-time delivery rate**.

**Final project, Rakamin Academy Data Science Bootcamp · 2022 · Group project (6 members)**

📊 **[View the full notebook →](notebooks/shipping-on-time-prediction.ipynb)** &nbsp;·&nbsp; 🎯 **[Stakeholder deck (PPTX) →](reports/final-presentation.pptx)**

---

## The problem

E-commerce shoppers don't come back when their packages arrive late:

- **69.7%** of customers are unlikely to return to a store if their order arrives late without notification
- **56%** of consumers switch to other stores when their regular store can't deliver on time
  *(source: shopify.co.id, 2020)*

Of the 10,999 shipping records in the dataset, **59.7% of packages were arriving late** — a churn problem hiding inside an operations problem.

**Goal:** Predict, at the point of dispatch, whether a package will arrive late, so the business can intervene (re-route, upgrade shipping, notify the customer) before the delay happens.

---

## Approach

| Stage | What we did |
|---|---|
| **EDA** | Distribution analysis, correlation heatmaps, category-vs-target plots across 12 features and 10,999 records |
| **Preprocessing** | IQR-based outlier capping, MinMax normalization on numeric features, label + one-hot encoding on categoricals |
| **Feature selection** | Dropped `ID`, `Customer_care_calls`, `Customer_rating` to prevent data leakage (post-purchase signals not available at dispatch time) |
| **Modelling** | Trained & tuned 6 classifiers: Logistic Regression, KNN, Decision Tree, Random Forest, Gradient Boosting, XGBoost |
| **Selection** | Chose the model with the **most stable AUC across train/test** to minimize overfitting risk in production |

---

## Results

Final model: **Logistic Regression** (tuned: `penalty=l2`, `C=0.1`, `solver=saga`)

| Metric | Score |
|---|---|
| Accuracy | 0.638 |
| Precision | 0.692 |
| Recall | **0.706** |
| F1 | 0.699 |
| ROC-AUC (test) | 0.721 |
| ROC-AUC (train) | 0.721 |

Why not the highest-accuracy model? Tree ensembles (Random Forest, XGBoost) scored marginally better on test accuracy but showed AUC train scores of 0.84–0.98 vs ~0.72 on test — classic overfitting. Logistic Regression's matching train/test AUC made it the safer bet for production deployment.

---

## The insight that mattered

Feature importance from the final model:

| Feature | Coefficient |
|---|---|
| `Discount_offered` | **+2.54** |
| `Weight_in_gms` | −1.62 |
| `Prior_purchases` | −0.58 |
| `Cost_of_the_Product` | −0.42 |

Heavy-discount, lightweight items were the strongest predictors of late delivery. Cross-referencing with the business context, this pointed to a **short-term sales promotion** the company had run shortly before the data was collected:
- Every customer received a special discount
- Promotion only applied to items < 5,000 grams
- New customers only

The promotion drove a volume spike of small, discounted packages that the existing fleet couldn't absorb — the late deliveries weren't random, they were the promotion's operational footprint.

---

## Business recommendations

1. **Redesign the discount program** — cap customer count, enforce per-tier limits, personalize by membership tier rather than blanket discounts
2. **Add third-party shipping partners** to surge capacity during peak promotional windows
3. **Optimize fleet utilization** during high-volume periods

**Projected impact:** +23.27% increase in on-time delivery rate after model implementation.

---

## Tech stack

`Python` · `pandas` · `NumPy` · `scikit-learn` · `XGBoost` · `statsmodels` · `seaborn` · `matplotlib` · `Jupyter`

---

## Repo guide

```
datascience-in/
├── README.md
├── requirements.txt
├── notebooks/
│   └── shipping-on-time-prediction.ipynb   ← full analysis end-to-end
├── reports/
│   └── final-presentation.pptx             ← stakeholder deck
└── data/
    └── README.md                            ← dataset source & schema
```

---

## My role

As the only team member with prior experience in the logistics industry, I took ownership of:

- **Framing the business problem** — translating a generic "predict late deliveries" brief into the customer-churn lens that anchors the README (the 69.7% / 56% stats and why on-time delivery is a retention problem, not just an operations problem).
- **Exploratory Data Analysis** — distribution, correlation, and category-vs-target work that exposed the discount/weight pattern later confirmed by the model's feature importance.
- **Business recommendations** — turning the model's coefficients into the three operational changes in the final deck (discount program redesign, third-party shipping partners, fleet utilization).

---

## Team

**Indra** · Refanie · Yanti · Fajar · Rahma · Handika

---

## Reproducing this analysis

```bash
git clone https://github.com/laksanaindra9/datascience-in.git
cd datascience-in
pip install -r requirements.txt
jupyter notebook notebooks/shipping-on-time-prediction.ipynb
```

Dataset (`shipping.csv`) is publicly available — see [`data/README.md`](data/README.md) for the source link.
