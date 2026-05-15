# analytics-portfolio
Professional portfolio for analytics, data science, and decision science work. 

# Analytics Portfolio

**Adam Zavala** · Analytics & Decision Science Leader  
[linkedin.com/in/adam-zavala](https://linkedin.com/in/adam-zavala)

---

## What This Portfolio Demonstrates

Applied analytics projects, each built around a real business question. The through-line across all of them: measurement should be designed before work begins, findings should connect to decisions rather than stop at descriptions, and honest constraints on what the analysis can and cannot claim are more valuable than polished conclusions that don't hold up to scrutiny.

The projects are ordered by methodological complexity, not recency. Together they cover causal inference, inferential regression, predictive classification, and applied LLM integration — the analytical stack most relevant to Director-level data science and decision science leadership roles.

---

## Projects

### [`causal-inference-did-psm`](./causal-inference-did-psm/)
**Did a minimum wage increase affect employment? | DiD + Propensity Score Matching**

Implements two foundational causal inference methods — Difference-in-Differences and Propensity Score Matching — on the Card & Krueger (1994) natural experiment and LaLonde/NSW job training dataset. Demonstrates pre-trend testing, propensity score overlap assessment, post-matching covariate balance (SMD), and ATT estimation against an experimental benchmark. The focus is on assumption transparency: each method's validity conditions are stated explicitly, and robustness checks document where the estimates are stable and where they are not.

**Key methods:** DiD, PSM, parallel trends test, overlap check, SMD balance, placebo test  
**Business question:** How do we measure the causal impact of an intervention when randomization wasn't possible?

---

### [`kpi-interpretation-tool`](./kpi-interpretation-tool/)
**LLM-assisted KPI interpretation for analytics teams | Claude API + Python**

A working analytics workflow tool that takes raw KPI metrics and returns structured natural-language interpretation across four layers: what happened, signal vs. noise, candidate explanations, and recommended actions. Built with the Anthropic Python SDK (`claude-sonnet-4-6`). Tested on three IT service operations KPIs with distinct signal patterns — strong trend, discrete spike, and late deterioration. Documents the prompt engineering decisions that produce consistent output structure across varied inputs.

**Key methods:** LLM API integration, structured prompting, four-question interpretation framework  
**Business question:** How do we reduce the distance between a metric and a decision-ready insight?

---

### [`ols-wage-regression`](./ols-wage-regression/)
**Returns to education and the gender wage gap | OLS regression with diagnostics**

Estimates a Mincer earnings equation on CPS 1985 labor market data (N=534). Demonstrates the full OLS workflow: model progression from naive baseline to full specification, log-coefficient interpretation with exact conversion, diagnostic checks (residuals vs. fitted, QQ plot, HC3 robust standard errors), robustness checks, and a three-part stakeholder translation framework. The key finding — each additional year of education is associated with 9.6% higher wages (95% CI: 7.9%–11.3%) — is presented alongside an explicit discussion of why this is an association rather than a causal estimate.

**Key methods:** OLS, log transformation, robust SEs, diagnostic plots, omitted variable bias  
**Business question:** How do we quantify workforce relationships in a way that is statistically defensible and useful to decision-makers?

---

### [`employee-attrition-classification`](./employee-attrition-classification/)
**Which employees are at risk of leaving? | Logistic regression + random forest**

Builds an attrition prediction model on a synthetic HR dataset (N=1,470, 16.7% attrition) and treats every modeling choice as a business decision. Accuracy is rejected as the primary metric — a majority-class predictor achieves 83% accuracy without identifying a single flight risk. The recommended model (logistic regression, 82% recall, AUC 0.888) is selected based on asymmetric misclassification costs. Threshold tuning is framed as a capacity planning tool: at threshold 0.40, 42 of 49 leavers are caught with 109 total conversations. Feature importance is compared across two methods (built-in Gini vs. permutation) with documented methodological reasons for preferring the latter.

**Key methods:** Binary classification, class imbalance handling, ROC-AUC, precision/recall tradeoff, threshold tuning, permutation importance  
**Business question:** How do we build a model that supports retention intervention decisions rather than just producing a prediction?

---

## Technical Stack

**Languages:** Python 3.13  
**Core libraries:** pandas · numpy · statsmodels · scikit-learn · matplotlib · seaborn  
**AI/LLM:** Anthropic Python SDK · claude-sonnet-4-6  
**Environment:** Jupyter Notebooks · VS Code

---

## A Note on Framing

Each notebook is structured to answer three questions a Director of Analytics would ask: why this problem, why this approach, and what does this change? Technical execution is necessary but not sufficient — the best projects are the ones where the analytical judgment layer is visible alongside the code.

The limitations sections in each README and notebook are not disclaimers. They are evidence of the analytical maturity required to use these methods responsibly in organizational contexts.
