# Crime Data Analysis: Population and Theft Rates in NZ

Does the population of a territorial authority (TA) affect its theft rate? A data science project using NZ Police and Stats NZ data, completed for a university course.

![Theft rate against log population](images/theft-vs-log-population.png)

## Summary
I investigated whether the population size of a TA is related to its recorded theft rate, using data for 67 TAs across three financial years (FY2023 to FY2025). Larger TAs tended to have higher theft rates, with a moderate positive correlation (pooled Pearson 0.62, 95% CI 0.52 to 0.70). The relationship held in each individual year (0.60 to 0.65). Population on its own was a weak predictor, explaining about a third of the variation (R² = 0.363).

## The question
Is there a significant correlation between TA population size and theft rate per 1,000 residents, and how well can population predict theft rate?

## Data
- **NZ Police, Victimisation Time and Place:** monthly counts of recorded victimisations by offence type and TA (891,389 rows). Theft made up 67.73% of recorded victimisations, which is why I focused on it.
- **Stats NZ, Subnational Population Estimates:** population of each TA at 30 June each year.

I reshaped the population data, standardised TA names so the two sources matched, grouped months into financial years, and calculated theft per 1,000 residents. Population was log-transformed because it was skewed.

Sources:
- [NZ Police, Victimisation Time and Place](https://www.police.govt.nz/about-us/publications-statistics/data-and-statistics/policedatanz/victimisation-time-and-place)
- [Stats NZ, Subnational population estimates](https://www.stats.govt.nz/information-releases/subnational-population-estimates-at-30-june-2025/)

## Method
1. Explored national and TA-level patterns in recorded victimisations
2. Measured the correlation between theft rate and log population (Pearson and Spearman)
3. Repeated the correlation for each year separately, because the 201 observations are 67 TAs tracked over three years and are not independent
4. Fitted a linear model and checked its assumptions with diagnostic plots
5. Log-transformed theft rate, which fixed the assumption problems
6. Tested predictive performance with 10-fold cross-validation against a simple baseline

## Results
| Measure | Result |
|---|---|
| Pooled Pearson correlation | 0.62 (95% CI 0.52 to 0.70) |
| Pooled Spearman correlation | 0.63 |
| Correlation by year | 0.60 to 0.65, all p < 0.001 |
| R² | 0.363 |
| Cross-validated RMSE | 12.56 (baseline 15.40) |

Population improved predictions over the baseline by 2.83 units, but left a lot of variation unexplained, so it is not a strong predictor on its own.

![Diagnostic plots after log transformation](images/diagnostics-log-model.png)
![K-fold cross-validation RMSE](images/kfold-rmse.png)

## Limitations
- Recorded crime only, so unreported crime is not captured
- Dwelling offences other than burglary are left out of the public data
- 201 observations come from 67 TAs over three years, so they are not fully independent
- Only three years of data
- A single predictor, so a lot of variation is unexplained
- Cross-validation results changed noticeably between folds, which shows sensitivity to which TAs land in each fold

Correlation does not show cause. Recorded rates should also not be read as a ranking of "safe" and "dangerous" areas, since reporting behaviour and policing activity affect recorded counts.

## My role
[Edit this to match what you did. From the report, it could include:]
- Completed the Phase 2 analysis: correlation testing, model diagnostics, log transformation and cross-validation
- Cleaned and joined the two datasets
- Wrote the report and produced the figures
- Planned the project in ProjectLibre

## Tools
R, R Markdown, ProjectLibre

## Files
- `analysis.Rmd`: [rename to match your file] the full analysis
- `report.pdf`: [if you decide to include it] the written report
- `images/`: figures used in this README

## Data, Privacy and Ethics
The datasets are publicly available from the sources above and are not included in this repo.

The Police data includes finer geographic detail (Area Unit), which can raise disclosure risk when counts are low and someone has local knowledge of the area. I aggregated everything to territorial authority level and did not produce or store results at a finer level. This reduces the risk of pointing to a specific person or property while keeping enough detail for the analysis. The repo contains no data finer than what is already publicly available from the Police and Stats NZ.
