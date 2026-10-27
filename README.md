# Fairness in Recidivism Prediction: ProPublica vs. Northpointe on COMPAS

This project audits **COMPAS**, a widely used criminal-justice risk-scoring algorithm, for racial bias. Using **5,278 defendants** (3,175 Black, 2,103 white), I tested both sides of a famous debate: ProPublica's claim that COMPAS's **errors** fall unequally on Black defendants, and Northpointe's rebuttal that the score is **calibrated** across races. Both turn out to be true — Black defendants who were not rearrested were **1.9x as likely to be wrongly labeled high-risk** (42.3% vs. 22.0%), even though scores predict rearrest similarly for both groups. Built for **MSBA 6341 — Responsible AI** (Carlson, University of Minnesota).

## 🔍 Project Goals

- Test empirically whether ProPublica's allegation (unequal error rates) and Northpointe's rebuttal (predictive parity) are supported by the data.
- Determine whether the racial gap in COMPAS labels is explained by legitimate risk factors (priors, age, sex, charge severity).
- Recommend how fairness should be defined and operationalized for high-stakes decisions like bail, sentencing, and parole.

## 📄 Files Included

- **`COMPAS_Fairness_Analysis.ipynb`** — reproducible analysis: base rates, error-rate audit, calibration by decile, logistic regressions, and robustness checks.
- **`report/Fairness_in_Recidivism_Prediction_Report.pdf`** — written report with position, ethical reasoning, policy recommendation, and appendix tables.
- **`docs/Case_Brief.pdf`** — the case prompt and data dictionary.
- **`data/compas-scores.csv`** — cleaned subsample of ProPublica's public COMPAS dataset.
- **`outputs/`** — error-rate and calibration charts.
- **`requirements.txt`** — Python dependencies.

## 🧪 Methods Used

- **Fairness metrics by group:** false-positive rate, false-negative rate, positive predictive value (PPV), and accuracy — treating a Medium/High label as a positive prediction.
- **Calibration analysis:** observed two-year rearrest rate by COMPAS decile score and race.
- **Logistic regression** (statsmodels) with parallel models — race → COMPAS label vs. race → actual rearrest — controlling for prior offenses, age, sex, and charge degree; odds ratios with 95% CIs.
- **Robustness checks** across three model specifications (full controls, no priors, race only).
- **Ethical frameworks:** the fairness impossibility result (Chouldechova, 2017; Kleinberg et al., 2017), Rawls's veil of ignorance, and Kant's respect for persons.

## 📈 Key Findings

- **ProPublica is right about errors:** among defendants *not* rearrested, **42.3%** of Black defendants were labeled Medium/High vs. **22.0%** of white defendants; among those who *were* rearrested, **49.6%** of white defendants were labeled Low vs. **28.5%** of Black defendants.
- **Northpointe is right about calibration:** PPV is similar (**65.0% vs. 59.5%**), and at decile 7 rearrest rates are nearly identical (**60.9% vs. 60.2%**).
- **The label gap isn't explained by the controls:** holding priors, age, sex, and charge constant, Black defendants had **1.62x the odds** of a Medium/High label (95% CI 1.42–1.86, p < .001) but only **1.10x the odds** of actual rearrest (not significant, p = .119). The label-vs-rearrest gap holds at **~1.5–1.6x** across all three specifications.
- **Both criteria can't hold at once:** with different base rates (**52.3% vs. 39.1%**), calibration and equal error rates are mathematically incompatible — so fairness is a policy choice, not a tuning problem.
- The outcome measures *rearrest*, which also reflects policing intensity — weakening the case that calibration alone proves fairness.

## 🛠 Technologies

- Python (pandas, NumPy, statsmodels, Matplotlib)
- Jupyter Notebook
- Algorithmic fairness metrics (FPR/FNR parity, predictive parity, calibration)
- Logistic regression & robustness analysis

## ✅ Outcome

A recommendation to prioritize **false-positive parity** wherever an error can cost someone their liberty: a COMPAS label should not independently justify detention or a harsher sentence unless an independent audit shows the false-positive-rate gap is below a **publicly defined threshold** — backed by recurring group-level audits, documented judicial use, and a defendant's right to examine and challenge the score.

---

By **Parul Chaudhary** · [LinkedIn](https://www.linkedin.com/in/parulchaud) · [Email](mailto:parul.jaswant@gmail.com)
