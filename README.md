# Does Climate Risk Data Improve Home Price Predictions?

A machine learning capstone comparing traditional real estate valuation models against models enhanced with climate vulnerability data, across three U.S. cities.

**[📄 Full paper (PDF)]() • [🎥 Recorded presentation](https://youtu.be/o1IISqVjLfk) • [💻 R code](./analysis.R)**

## TL;DR

I tested whether adding climate vulnerability data (heat, flooding, drought, and 20+ other risk factors) to standard home-price models actually improves prediction accuracy — or if it's mostly noise.

**Short answer: it depends where you are.**

- **Milwaukee:** Yes. Climate-enhanced models improved R² by ~0.04 and meaningfully reduced prediction error (XGBoost was the best-performing model, R² > 0.75).
- **Houston & Denver:** Inconclusive. The direction was similar but not statistically significant, most likely due to smaller sample sizes (Denver had only 108 usable census tracts).

This suggests climate risk factors are already showing up as real, measurable signals in some housing markets — just not detectable everywhere yet with the data available.

## Why this matters

Most existing research on climate and housing looks at a single hazard (usually flooding) or takes a national-level view. This project instead:
- Combined **20+ climate risk indicators** (not just flood risk) into a single comparison
- Focused on the **census-tract level**, which is the resolution actually useful to a homebuyer, real estate agent, or local zoning office
- Tested the same approach across three cities with very different climate exposure, to see if one model generalizes or if this has to be built city-by-city (it's the latter)

## What I did

1. **Combined three data sources:** Zillow/Realtor property listings (Kaggle), the U.S. Climate Vulnerability Index, and Census/ACS demographic data — merged at the census tract level via spatial joins.
2. **Engineered features** that mattered for local pricing: distance to city center, population density, a combined walkability/bikability score, and unemployment rate.
3. **Built and tuned four models per city:** linear regression baseline, elastic net, random forest, and XGBoost, using nested (double) cross-validation so hyperparameter tuning didn't leak into performance estimates.
4. **Tested significance**, not just direction — a paired t-test on squared errors, so "climate data helped" is a statistical claim, not just a chart that looks nicer.
5. **Used SHAP analysis** to identify which specific Milwaukee neighborhoods are most exposed to climate-driven home value risk, and which climate factors (overall CVI score, more than any single hazard) mattered most.

## Tools

`R` · `caret` · `glmnet` · `randomForest` · `xgboost` · `SHAPforxgboost` · `tidycensus` · `sf` (spatial joins)

## Full technical writeup

The complete methodology — including data cleaning decisions, outlier handling, multicollinearity checks, and how to run the code yourself — is documented in [`METHODOLOGY.md`](./METHODOLOGY.md) and in the [full paper](#).

---
*This was my capstone project for the MS in Data Science program at UW–La Crosse.*
