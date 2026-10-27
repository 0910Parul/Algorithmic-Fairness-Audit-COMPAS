# Auditing Algorithmic Fairness in Recidivism Risk Prediction

## Project Overview

This project audits racial disparities in COMPAS risk classifications using a **5,278-defendant** sample. It examines the distinction between unequal error rates, predictive parity, and calibration, then uses logistic regression to compare associations with risk labels and observed outcomes.

The analytical question is how the choice of fairness metric changes the assessment of a decision-support system. The sample includes **3,175 Black defendants** and **2,103 white defendants**. The outcome is **rearrest within two years**, rather than proven reoffending; a **Medium or High** COMPAS label is treated as a positive risk prediction.

## Key Highlights

- **Error disparity:** the saved analysis records false-positive rates of **42.3% for Black defendants** and **22.0% for white defendants**, a **1.92× ratio**.
- **Multiple fairness views:** compares false-positive and false-negative rates, positive predictive value (PPV), accuracy, and observed rearrest rates by score decile.
- **Adjusted analysis:** with the specified controls, the odds ratio for a Medium/High label was **1.62**, compared with **1.10** for observed rearrest.
- **Decision support:** translates the findings into a case-study recommendation for explicit fairness objectives, recurring audits, and human review.

See the [analysis notebook](COMPAS_Fairness_Analysis.ipynb), [report](report/Fairness_in_Recidivism_Prediction_Report.pdf), and [results evidence](docs/RESULTS_EVIDENCE.md).

## Technical Implementation

1. **Define the audit:** derive a Medium/High indicator and identify the two racial groups represented in the supplied dataset.
2. **Inspect base rates:** summarize group counts, observed rearrest rates, and the share receiving a Medium/High label.
3. **Measure error rates:** calculate FPR, FNR, PPV, and accuracy within each group using the same classification rule.
4. **Inspect calibration:** compare observed rearrest rates at each COMPAS decile and retain the group sample sizes.
5. **Fit parallel regressions:** model the risk label and observed rearrest separately, controlling for prior offenses, age category, sex, and charge degree; report race-associated odds ratios and 95% confidence intervals.
6. **Check robustness:** compare full controls, a specification without priors, and a race-only specification.

## Methods & Technologies

- **Python:** pandas, NumPy, Matplotlib, and statsmodels.
- **Statistical analysis:** group-conditioned classification metrics, score-bin comparisons, logistic regression, confidence intervals, and specification checks.
- **Communication:** comparison charts and a written discussion of competing fairness objectives and the limitations of the outcome measure.
- **Environment:** Jupyter Notebook; dependencies in [requirements.txt](requirements.txt).

## Results & Insights

The table summarizes saved notebook outputs. The group labels below correspond to `African-American` and `Caucasian` in the supplied CSV.

| Metric | Black defendants | White defendants |
|---|---:|---:|
| Sample size | 3,175 | 2,103 |
| Observed two-year rearrest rate | 52.3% | 39.1% |
| False-positive rate: Medium/High among those not rearrested | **42.3%** | **22.0%** |
| False-negative rate: Low among those rearrested | 28.5% | 49.6% |
| Positive predictive value: rearrest among Medium/High labels | 65.0% | 59.5% |
| Classification accuracy | 64.9% | 67.2% |

- **Different errors fall on different groups:** Black defendants who were not rearrested were more often labeled Medium/High; white defendants who were rearrested were more often labeled Low.
- **Calibration requires qualification:** observed rates were close at decile 7 (**60.9% versus 60.2%**), but the full table contains gaps at other deciles and varying sample sizes. PPVs also differ by 5.5 percentage points. These descriptive comparisons do not establish exact calibration or predictive parity.
- **Adjusted associations differ by outcome:** the full-control label model gives an odds ratio of **1.62 (95% CI 1.42–1.86)**; the rearrest model gives **1.10 (95% CI 0.98–1.25; p=0.119)**. The first association remains after the included controls; this does not establish its cause.
- **Robustness has a defined scope:** ratios of the two race-associated odds ratios range from **1.47 to 1.61** across the three specifications. They describe the fitted models, rather than a causal effect or formal test that the coefficients differ.

### Result scope

This audit covers the supplied two-group sample and one observed outcome. Rearrest is an imperfect proxy influenced by factors beyond individual behavior. Observational controls and descriptive calibration plots cannot establish fairness for every group, context, or use. The documentation update preserves the original analysis and does not claim a new model deployment or policy intervention.

## Repository Structure

| Path | Purpose |
|---|---|
| [COMPAS_Fairness_Analysis.ipynb](COMPAS_Fairness_Analysis.ipynb) | Group metrics, calibration, regressions, and robustness analysis |
| [data/compas-scores.csv](data/compas-scores.csv) | Supplied 5,278-defendant sample |
| [docs/Case_Brief.pdf](docs/Case_Brief.pdf) | Case context and data dictionary |
| [report/Fairness_in_Recidivism_Prediction_Report.pdf](report/Fairness_in_Recidivism_Prediction_Report.pdf) | Written interpretation and case-study policy recommendation |
| [outputs/error_rates_by_race.png](outputs/error_rates_by_race.png) | Error-rate comparison chart |
| [outputs/calibration_by_decile.png](outputs/calibration_by_decile.png) | Rearrest-rate comparison by score decile |
| [requirements.txt](requirements.txt) | Python dependencies |
| [docs/RESULTS_EVIDENCE.md](docs/RESULTS_EVIDENCE.md) | Sources, metric definitions, and interpretation boundaries |

## Getting Started

From the repository root:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
jupyter notebook COMPAS_Fairness_Analysis.ipynb
```

Run the notebook in order with the repository root as the working directory. The data is included; no API credentials are needed. Chart cells write to the existing `outputs/` directory. Dependencies are unpinned, so package behavior may differ from the saved environment.

## Applications & Takeaways

The project demonstrates how an analyst can audit an existing scoring system rather than assess it through aggregate accuracy alone. Useful practices include defining the positive prediction, showing group-level denominators, comparing harmful errors, and stating what the outcome actually measures.

The report proposes prioritizing false-positive parity in liberty-affecting decisions, supported by independent review, recurring audits, documented use, and a way to challenge scores. This is a case-study recommendation; no implementation or downstream impact was evaluated. The broader governance lesson is to make the decision objective and acceptable trade-offs explicit before adopting a score.

## Project Context

Developed for **MSBA 6341 — Responsible AI**, Carlson School of Management, University of Minnesota. The analysis examines the ProPublica–Northpointe debate using the supplied COMPAS dataset. Original data, code, charts, and report are retained.

---

By **Parul Chaudhary** · [LinkedIn](https://www.linkedin.com/in/parulchaud) · [Email](mailto:parul.jaswant@gmail.com)
