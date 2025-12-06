# DS785 Capstone Project: Climate Vulnerability and Real Estate Values

**Author:** Anne Roesler  
**Course:** DS785 Capstone

## Project Overview

This project investigates the relationship between climate vulnerability indicators and real estate values across three U.S. cities: Milwaukee, Houston, and Denver. Using machine learning techniques, the analysis compares predictive models with and without climate vulnerability data to assess whether climate factors improve home value predictions.

## Research Question

Does incorporating climate vulnerability data improve the prediction of median home values at the census tract level compared to models using only traditional real estate and demographic features?

## Data Sources

### Required Input Files

1. **Real Estate Data**
   - File: `realtor-data.zip.csv.zip`
   - Source: Kaggle real estate dataset
   - Contains: Property listings with attributes (bedrooms, bathrooms, lot size, square footage, ZIP codes)

2. **Climate Vulnerability Index (CVI) Data**
   - File: `Master CVI Dataset - Oct 2023.xlsx`
   - Sheets used:
     - Sheet 1: Domain-level vulnerability scores
     - Sheet 2: Indicator-level values
     - Sheet 3: Native units for climate indicators
   - Contains: Climate vulnerability metrics by census tract (heat, precipitation, flooding, extreme events)

3. **Census Data**
   - Retrieved via `tidycensus` package
   - Variables: Median income, population density, unemployment rate, median home value, median year built

## Study Areas

- **Milwaukee, WI** (Milwaukee County)
- **Houston, TX** (Harris County)
- **Denver, CO** (Denver County)

## Methodology

### Data Processing

1. **Geographic Aggregation**: Property-level data aggregated to census tracts using spatial joins
2. **Feature Engineering**: 
   - Modal bedrooms/bathrooms per tract
   - Median lot size and square footage per tract
   - Distance to central business district (CBD)
   - Combined walkability/bikability score
3. **Data Cleaning**: Outlier removal, missing value imputation
4. **Model Comparison**: Two model versions per city
   - Model 1: Real estate and demographic features only
   - Model 2: Model 1 + climate vulnerability indicators

### Modeling

- **Baseline**: Linear regression with preprocessing (centering, scaling, Yeo-Johnson transformation)
- **Advanced Models**:
  - Elastic Net (glmnet)
  - Random Forest
  - XGBoost
- **Validation Strategy**: Double cross-validation (nested CV)
  - Outer loop: 10-fold CV for model assessment
  - Inner loop: 5-fold CV for hyperparameter tuning
- **Model Selection**: Best model chosen based on RMSE within each outer fold

### Statistical Testing

Paired t-tests on squared errors to determine if climate data significantly improves predictions.

## Software Requirements

### R Version
- R >= 4.0.0

### Required Packages

```r
# Data manipulation and spatial analysis
library(tidyverse)
library(sf)
library(tidygeocoder)
library(geosphere)
library(stringr)

# Census and geographic data
library(tidycensus)
library(zipcodeR)

# Data import
library(readxl)
library(readr)

# Modeling and machine learning
library(caret)
library(MASS)
library(glmnet)
library(xgboost)
library(randomForest)

# Model interpretation
library(SHAPforxgboost)

# Visualization
library(ggplot2)
library(GGally)
library(gridExtra)
library(patchwork)

# Utilities
library(DescTools)
```

### Running the Analysis

The script is organized into sections that should be run sequentially:

1. **Data Reading and Processing** (Lines 1-250)
   - Loads and cleans all datasets
   - Creates city-specific data frames
   - Spatial aggregation to census tracts

2. **Train/Test Split Creation** (Lines 251-350)
   - Creates 80/20 splits for each city and model version

3. **Baseline Linear Regression** (Lines 351-450)
   - Establishes baseline performance
   - 10-fold repeated cross-validation

4. **Hyperparameter Tuning** (Lines 451-650)
   - Initial grid searches for XGBoost and Random Forest
   - Iterative refinement of hyperparameters
   - **Note**: Uncomment final tune grids once parameters are selected

5. **Double Cross-Validation** (Lines 651-850)
   - Nested CV for model selection and assessment
   - Generates final performance metrics
   - **Note**: Update `df_to_use`, random forest tune grid and XGBoost tune grid, and uncomment appropriate RMSE/R², residual, and sq-error lines

6. **Model Assessment** (Lines 851-900)
   - Statistical testing (paired t-tests)
   - Residual diagnostics
   - Visualization of model performance

7. **Final Model Fitting** (Lines 901-950)
   - Refit best models on complete datasets
   - Variable importance analysis
   - SHAP value computation

## Output Interpretation

### Performance Metrics

- **RMSE**: Root Mean Squared Error (in dollars) - lower is better
- **R²**: Proportion of variance explained - higher (closer to 1) is better
- **P-value**: Statistical significance of climate data improvement

### Model Comparison

For each city, compare:
- Model 1 RMSE vs. Model 2 RMSE
- P-value < 0.05 indicates significant improvement with climate data

## References

- Portions of spatial join code adapted from Claude AI (Sonnet 4.5) suggestions, September 29, 2025
- Census data accessed via tidycensus package
- Real Estate data source: Sakib, A. S. (2024). USA real estate dataset [Data set]. Kaggle. https://www.kaggle.com/datasets/ahmedshahriarsakib/usa-real-estate-dataset
- CVI data source: Tee Lewis, P. G., Chiu, W. A., Nasser, E., Proville, J., Barone, A., Danforth, C., Kim, B., Prozzi, J., & Craft, E. (2023). Characterizing vulnerabilities to climate change across the United States. Environment International, 172, 107772. https://doi.org/10.1016/j.envint.2023.107772

---

**Last Updated:** December 2025
