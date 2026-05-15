# KPI Interpretation Tool

A Python notebook that uses the **Claude API** to automate four-layer KPI narrative generation for IT service operations metrics.

Built as Portfolio Artifact 1 in a Director-track analytics leadership skill development plan. The tool demonstrates applied LLM integration in an analytical workflow — not just using the chat interface, but building programmatically with the API.

---

## What It Does

The tool takes a KPI time series (metric name, date, value) and returns a structured, decision-ready interpretation across four layers:

| Layer | Question | Output |
|---|---|---|
| **What happened** | Describe the trend | Direction, magnitude, duration — specific numbers required |
| **Signal vs. noise** | Is this meaningful? | Verdict: `STRONG SIGNAL` / `MODERATE SIGNAL` / `NOISE` |
| **Candidate explanations** | Why might this be happening? | Ordered plausible mechanisms, most to least likely |
| **Recommended action** | What should we do? | Specific next step with named threshold and decision trigger |

This four-layer framework maps directly to a measurement philosophy: analytics should answer four questions — what happened, why it happened, what's likely next, and what we should do about it. A report that stops at "the metric went up" hasn't finished the job.

---

## Demo

The notebook runs on three synthetic IT service operations KPIs across 12 weeks, each representing a different signal pattern:

| KPI | Pattern | Signal verdict |
|---|---|---|
| Zero Touch Rate | Strong upward trend (+19pp over 12 weeks) | 🔴 STRONG SIGNAL |
| Weekly Ticket Volume | Mid-series spike (+35% in weeks 4–6) then recovery | 🟡 MODERATE SIGNAL |
| Closed Within 1 Weekday Rate | Stable baseline then late deterioration (weeks 9–12) | 🔴 STRONG SIGNAL |

The ticket volume and resolution rate patterns are the harder interpretive tests — a well-designed prompt needs to distinguish a discrete anomaly from a trend, and catch a late dip in a stable series without averaging it away. Both pass on the first prompt attempt.

---

## Structure

```
Section 1 — Setup
  Cell 1: Imports, dotenv, API client initialization

Section 2 — Data
  Cell 2: Synthetic KPI dataset (3 metrics × 12 weeks)

Section 3 — Core Tool
  Cell 3: Development note — single-KPI quality gate (markdown)
  Cell 3: System prompt + interpret_kpi() function
  Cell 4: Full interpretation loop across all KPIs

Section 4 — Output
  Cell 5: Formatter functions (signal badge, markdown renderer)
  Cell 6: Terminal report (v2, with corrected bullet formatting)
           + markdown export to kpi_report.md

Section 5 — What This Demonstrates
  Markdown cell: Design decisions, technical signals, interview narrative
```

---

## Prompt Engineering Decisions

**Pre-computed summary statistics in the user message**
The user message passes baseline average, recent average, and percentage change before asking for interpretation. This reduces the model's arithmetic burden and produces more consistent quantitative references in the output — the model cites the numbers you computed rather than deriving its own.

**Signal verdict enforced in the system prompt**
The signal classification (`STRONG SIGNAL` / `MODERATE SIGNAL` / `NOISE`) is required as part of the `signal_vs_noise` field rather than inferred post-hoc. This means the model commits to a classification as part of its reasoning, not as a label applied afterward. The `format_signal_badge()` function then extracts the verdict and maps it to an emoji badge for display.

**JSON-only output with no preamble**
The system prompt instructs the model to return only valid JSON with exactly four fields. A code fence stripping step handles cases where the model wraps the JSON in markdown backticks, which some model versions do inconsistently.

**Ordered explanations**
The candidate explanations prompt specifies "ordered from most to least likely" — without this instruction, models tend to list mechanisms in arbitrary order. Ordering forces the model to make a judgment call about probability rather than list-dumping.

---

## Requirements

```bash
pip install anthropic python-dotenv pandas numpy
```

**API key setup:**
Create a `.env` file in the project root:
```
ANTHROPIC_API_KEY=your-key-here
```

The notebook loads this automatically via `python-dotenv`. Never commit `.env` to version control — it is included in `.gitignore`.

**Tested on:** Python 3.13.5 | Model: `claude-sonnet-4-6`

---

## Running the Notebook

```bash
jupyter notebook kpi_interpretation_tool.ipynb
```

Run cells sequentially — Cell 4 (interpretation loop) depends on variables defined in Cells 1–3. Cell 6 depends on formatter functions defined in Cell 5.

**Expected runtime:** ~15–20 seconds for the full three-KPI loop (one API call per KPI).

---

## Output Formats

**Terminal report** — rendered inline in Jupyter with signal badges, word-wrapped text, and structured section headers.

**Markdown report** — exported to `kpi_report.md`, GitHub-renderable, suitable for sharing or embedding in documentation.

---

## Dataset Note

The KPI dataset is synthetic — generated from distributional parameters that mimic real IT service operations metrics. It is not based on proprietary data. The three KPIs (Zero Touch Rate, Weekly Ticket Volume, Closed Within 1 Weekday Rate) are drawn from the same operational domain as two quasi-experimental studies described in the notebook's final markdown cell.

---

## Related Work

This artifact is part of a broader causal inference and applied analytics portfolio:

- **DiD + PSM Implementation** — Difference-in-Differences on Card-Krueger (1994) and Propensity Score Matching on LaLonde/NSW (1986): [`causal-inference-did-psm`](https://github.com/adamzavala/causal-inference-did-psm)

---

## References

Anthropic. (2026). *Claude API documentation.* [https://docs.anthropic.com](https://docs.anthropic.com)

Zavala, A. (2026). *Analytics Leadership Philosophy & Career Vision.* Internal document.
