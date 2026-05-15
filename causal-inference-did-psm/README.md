# Causal Inference Implementation: DiD and PSM

Implementations of **Difference-in-Differences (DiD)** and **Propensity Score Matching (PSM)** on the two canonical causal inference datasets in applied econometrics.

Built as part of a structured causal inference methods study track, with explicit connections between textbook implementations and real-world analytical design decisions.

---

## Notebooks

| File | Purpose |
|---|---|
| `causal_inference_did_psm_ARTIFACT.ipynb` | Clean portfolio notebook — concise code with methodology context |
| `causal_inference_did_psm_LEARNING.ipynb` | Extended study version — detailed methodology commentary, individual cells per logical step, and explicit connection between implementations and real study designs |

Start with the artifact version for a clean read. Use the learning version as a study reference.

---

## What This Covers

### Sections 1–3: Difference-in-Differences (Card-Krueger 1994)

Card and Krueger's minimum wage study is the canonical DiD application. New Jersey raised its minimum wage in April 1992; Pennsylvania did not. 410 fast food restaurants were surveyed before and after.

**Implemented:**
- Visual pre-trend inspection (parallel trends check)
- Manual 2×2 DiD estimator: `(NJ_post − NJ_pre) − (PA_post − PA_pre)`
- Regression formulation with interaction term: `fte ~ treated + post + treated:post`
- Empirical confirmation that the interaction coefficient is algebraically identical to the manual 2×2 estimate (difference = 0.000000 to six decimal places)
- Covariate model with chain fixed effects and starting wage
- Within-pre-period placebo test

**Key result:** The `treated:post` interaction coefficient absorbs only the variation that is simultaneously in the treated group *and* the post period — after differencing out the baseline level gap (`treated`) and the shared time trend (`post`). This is the DiD estimator in regression form, and it is structurally identical to the 2×2 subtraction.

---

### Sections 4–5: Propensity Score Matching (LaLonde/NSW 1986, Dehejia & Wahba 1999)

The LaLonde dataset — 185 NSW job training participants (treated) and 2,490 CPS observational controls — is the canonical dataset for demonstrating selection bias and the limits of observational matching methods. It has a known experimental benchmark (~$1,794 ATT) from the randomized study, which allows direct evaluation of how well PSM recovers the true causal effect.

**Implemented:**
- Raw covariate imbalance assessment (standardized mean differences)
- Propensity score estimation via logistic regression
- Overlap check with common support trimming — **the step most PSM tutorials skip**
- 1-to-1 nearest-neighbor matching on propensity score (with replacement)
- Post-matching covariate balance assessment (SMD table + balance plot)
- ATT estimation compared to the LaLonde experimental benchmark

**Key results:**
- 79.9% of the dataset falls outside the common support region — matching without trimming would extrapolate rather than compare
- PSM guarantees balance on the propensity score, not on individual covariates — the post-matching SMD table is mandatory, not optional
- PSM reduces the naive comparison bias (~$10,400 gap) but does not fully recover the experimental benchmark — the residual gap reflects selection on unobservable characteristics

---

### Section 6: Synthesis

Connects both implementations to real analytical decisions in observational study design:

- When DiD is more defensible than a before/after quasi-experiment, and what makes the parallel trends assumption testable in ways the before/after no-confounders assumption is not
- What the LaLonde overlap check demonstrates about common support as a prerequisite for valid matching
- Why PSM with a richer covariate set reduces but does not eliminate residual bias when selection is driven by unobserved characteristics
- The honest claim that survives both implementations: matching reduces selection bias from observed confounders; unobserved confounder risk is real, named, and structurally uncloseable without randomization or a credible instrument

---

## Dataset Note

> ⚠️ **Both datasets are reconstructed from published summary statistics**, not loaded from the original microdata, due to environment constraints (package incompatibilities and network restrictions on the original data sources).
>
> Distributional properties are calibrated to:
> - **Card-Krueger:** Table 2 means and standard deviations (American Economic Review, 1994)
> - **LaLonde/NSW + CPS:** Table 2 means from Dehejia & Wahba (1999)
>
> Individual-level observations are synthetic. **Point estimates differ from published results** (Card-Krueger: +4.76 FTE vs. published +2.75 FTE; LaLonde PSM bias reduction: ~15% vs. Dehejia & Wahba's higher recovery rate). **The methods are identical to those applied to the original microdata.** The numbers are illustrative.
>
> To replicate with original data: Card-Krueger data is available at [David Card's Berkeley faculty page](https://davidcard.berkeley.edu/data_sets.html). LaLonde/NSW data is available through Dehejia's replication files.

---

## Requirements

```bash
pip install pandas numpy matplotlib seaborn statsmodels scikit-learn
```

No external data downloads required — datasets are constructed inline from published summary statistics.

**Tested on:** Python 3.13.5

---

## Running the Notebook

```bash
jupyter notebook causal_inference_did_psm_ARTIFACT.ipynb
```

Run cells sequentially — later sections depend on variables defined in Section 1 (dataset construction) and Section 4 (propensity score estimation and trimming).

---

## References

Card, D., & Krueger, A. B. (1994). Minimum wages and employment: A case study of the fast-food industry in New Jersey and Pennsylvania. *American Economic Review, 84*(4), 772–793.

Dehejia, R. H., & Wahba, S. (1999). Causal effects in nonexperimental studies: Reevaluating the evaluation of training programs. *Journal of the American Statistical Association, 94*(448), 1053–1062.

LaLonde, R. J. (1986). Evaluating the econometric evaluations of training programs with experimental data. *American Economic Review, 76*(4), 604–620.

Cunningham, S. (2021). *Causal inference: The mixtape.* Yale University Press. [https://mixtape.scunning.com](https://mixtape.scunning.com)

Facure Alves, M. (2022). *Causal inference for the brave and true.* [https://matheusfacure.github.io/python-causality-handbook](https://matheusfacure.github.io/python-causality-handbook)
