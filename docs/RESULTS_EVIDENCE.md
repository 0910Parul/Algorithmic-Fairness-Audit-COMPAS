# Results Evidence and Audit Boundaries

The primary source is [COMPAS_Fairness_Analysis.ipynb](../COMPAS_Fairness_Analysis.ipynb), with [data](../data/compas-scores.csv) and the [written report](../report/Fairness_in_Recidivism_Prediction_Report.pdf). Cell positions below are **1-based**, including markdown.

| Claim | Notebook evidence |
|---|---|
| N=5,278; group sizes 3,175 and 2,103 | Cells 2 and 4 |
| Positive risk prediction | Cell 2: `score_text != "Low"`; supplied labels are Low, Medium, and High |
| Base rates 52.3% and 39.1% | Cell 4 |
| FPR 42.3% and 22.0%; FNR 28.5% and 49.6% | Cell 6 |
| PPV 65.0% and 59.5%; accuracy 64.9% and 67.2% | Cell 6 |
| FPR ratio 1.92 | Cell 7 |
| Decile-7 rearrest rates | Cells 9–10: 209/343 for Black defendants and 68/113 for white defendants |
| Full-control odds ratios | Cells 12–13: label OR 1.625 (CI 1.42–1.86); rearrest OR 1.103 (CI 0.98–1.25; p≈0.119) |
| Ratio of race-associated ORs across specifications | Cells 12–13: 1.473, 1.572, 1.607 |

## Interpretation boundaries

- FPR conditions on no observed rearrest; FNR conditions on observed rearrest; PPV conditions on a Medium/High label. These denominators are not interchangeable.
- Calibration-by-decile and PPV are distinct summaries. Similarity at a selected decile does not prove parity across all scores or groups. Cell 9 includes, for example, a larger gap at decile 10 with only 50 white defendants in that bin.
- The regressions are observational. A remaining association after these controls does not identify its mechanism, and a p-value above 0.05 does not prove that an association is absent.
- The odds-ratio quotient compares two different outcome models. It is a descriptive robustness summary, not a causal disparity measure or a formal coefficient-difference test.
- The audit is limited to the supplied sample's two racial groups and the recorded two-year rearrest outcome. It does not measure proven reoffending or generalize automatically to another setting.
- The report's policy recommendation is an analytical conclusion from the case study, not evidence that a policy was implemented or improved outcomes.

The documentation review inspects saved analysis and source data; no code, chart, dataset, or report is modified.
