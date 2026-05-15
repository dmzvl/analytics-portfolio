# OLS Regression: Returns to Education and the Gender Wage Gap

## Business Question

What is the association between years of education and hourly wages — and how precisely can we estimate it in a way that is defensible to a skeptical executive and actionable for compensation strategy decisions?

## Key Finding

Each additional year of education is associated with **9.6% higher hourly wages** (95% CI: 7.9%–11.3%), after controlling for work experience and gender. Returns to experience peak at approximately 36 years before declining, consistent with the Mincer (1974) human capital model. Women in this sample earned **22.7% less per hour** than men with equivalent education and experience — a statistically significant gap that is not fully explained by the variables available in this dataset. An organization investigating pay equity should extend this analysis with controls for occupation, industry, and hours worked before drawing conclusions.

## Methodology

We estimate a Mincer earnings equation using OLS regression on the CPS 1985 dataset (N=534 workers, Berndt 1991). Log-transforming wages is standard in labor economics — it compresses right-skew and produces coefficients interpretable as percentage wage changes. We include experience-squared to capture the documented concavity of experience-earnings profiles; a linear experience specification is explicitly rejected because it cannot represent diminishing returns and produces worse model fit. Diagnostic checks (residuals vs. fitted, QQ plot, HC3 robust standard errors) are performed and documented. A robustness check confirms the condition number warning in the base model reflects numerical instability from the polynomial experience term, not conceptual misspecification — coefficients are identical under mean-centered specification.

**Critical limitation:** This is a conditional association study, not a causal study. Selection into education on unobservables (ability, family background) likely inflates the education coefficient. A natural experiment is required for causal identification.

## Technical Requirements

```
Python 3.8+
numpy
pandas
statsmodels
matplotlib
seaborn
```

Data loads directly from a public URL — no local files required:
```python
url = "https://vincentarelbundock.github.io/Rdatasets/csv/AER/CPS1985.csv"
```

## How to Run

```bash
pip install numpy pandas statsmodels matplotlib seaborn
jupyter notebook ols_wage_regression.ipynb
```

Run all cells top to bottom. The notebook is designed to run clean from a fresh kernel with no hidden state.

## Limitations

1. **No causal identification.** The education coefficient is a conditional association. Unobserved factors correlated with both education and wages (ability, family background, geography) bias the estimate upward. A tuition reimbursement investment case requires stronger causal evidence than OLS on observational data can provide.

2. **Incomplete gender gap model.** Occupation, industry, hours worked, and negotiating behavior are not controlled. The 22.7% conditional gap is not a discrimination estimate — it is a residual gap after controlling only for education and experience. A pay equity audit requires substantially more controls.

3. **1985 cross-sectional data.** Returns to education and the gender wage gap have evolved materially in the decades since. These estimates describe a specific labor market moment and should not be extrapolated to current conditions without validation.

---

## Repository Context

Part of an analytics portfolio demonstrating applied decision science skills for Director and Lead IC roles.

Related artifacts:
- [`causal-inference-did-psm`](https://github.com/dmzvl/causal-inference-did-psm) — DiD and PSM causal inference implementations
- [`employee_attrition_classification.ipynb`](./employee_attrition_classification.ipynb) — Binary classification with imbalanced data

---

*Reference: Berndt, E.R. (1991). The Practice of Econometrics. Addison-Wesley. | Mincer, J. (1974). Schooling, Experience, and Earnings. Columbia University Press.*
