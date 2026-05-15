# Employee Attrition Prediction: Classification as a Decision-Support Problem

## Business Question

Which employees are at highest risk of leaving — and at what model confidence threshold should a manager initiate a retention conversation, given realistic capacity constraints and the asymmetric cost of a missed leaver versus an unnecessary conversation?

## Key Finding

A logistic regression model achieves **82% recall on the attrition class** (catching 40 of 49 actual leavers in the held-out test set), with ROC-AUC of 0.888. A random forest achieves higher AUC (0.947) but only 65% recall due to extreme conservatism — it generates zero false alarms but silently misses 17 real leavers. Given that a missed leaver (recruiting, onboarding, productivity loss) costs significantly more than one unnecessary retention conversation, **logistic regression is the recommended model**. At threshold 0.40, 42 of 49 leavers are caught with 109 total conversations — the same recall as threshold 0.30 but with 23 fewer conversations, making it the dominant operating point. OverTime is the strongest attrition predictor (4x higher odds), and EnvironmentSatisfaction is the strongest protective factor (43% lower odds) — though these are associations, not causal levers.

## Methodology

We train and compare logistic regression (interpretable baseline) and random forest (nonlinear ensemble) on a synthetic HR attrition dataset (N=1,470, 16.7% attrition). Class imbalance is addressed via `class_weight='balanced'` rather than SMOTE — avoiding synthetic data artifacts while appropriately upweighting the minority class during training. Accuracy is explicitly rejected as the primary metric: a majority-class predictor achieves 83% accuracy without identifying a single flight risk. Primary metrics are recall on the attrition class and ROC-AUC. Feature importance is assessed using both built-in Gini impurity and permutation importance (test set, ROC-AUC scoring) — methodological differences are documented and permutation importance is preferred for generalization assessment. Threshold sensitivity is analyzed from 0.30 to 0.60 as a capacity planning tool.

**Critical limitation:** Feature importance identifies predictive associations, not causal relationships. Intervening on top-ranked features (e.g., reducing overtime) will not necessarily reduce attrition without an intervention study confirming the causal direction.

## Technical Requirements

```
Python 3.8+
numpy
pandas
scikit-learn
matplotlib
seaborn
```

No external data files required. Dataset is generated reproducibly via:
```python
from sklearn.datasets import make_classification
np.random.seed(42)
```

## How to Run

```bash
pip install numpy pandas scikit-learn matplotlib seaborn
jupyter notebook employee_attrition_classification.ipynb
```

Run all cells top to bottom. The notebook runs clean from a fresh kernel with no hidden state. All randomness is seeded for full reproducibility.

## Limitations

1. **Synthetic data.** The dataset is generated via `make_classification` to match IBM HR Analytics characteristics (N=1,470, ~16.7% attrition). Real HR data contains occupational clustering, manager effects, and nonlinear tenure patterns not captured in the synthetic generative process. Model performance on real data may differ.

2. **Predictive, not causal.** Feature importance identifies which variables are correlated with attrition — not which variables cause it. OverTime_encoded is the strongest predictor, but overtime may be a proxy for upstream conditions (staffing levels, role design, manager behavior) rather than a direct lever. An intervention study is required before committing resources to any feature-based retention program.

3. **No longitudinal validation.** The model is evaluated on a held-out random split from the same dataset. Production deployment requires temporal validation — training on historical data and evaluating on a future cohort — to confirm the model generalizes across time, not just across random splits.

---

## Repository Context

Part of an analytics portfolio demonstrating applied decision science skills for Director and Lead IC roles.

Related artifacts:
- [`causal-inference-did-psm`](https://github.com/dmzvl/causal-inference-did-psm) — DiD and PSM causal inference implementations
- [`ols_wage_regression.ipynb`](./ols_wage_regression.ipynb) — Multivariate OLS with diagnostics and stakeholder translation
