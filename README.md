# Geographical Medicare Analysis: Predicting Regional Per Capita Spending

## Background and Overview

Federal policymakers, health economists, and medical researchers have long recognized that the volume, cost, and quality of healthcare services received by Medicare beneficiaries vary substantially across different geographic regions of the United States. To understand these regional disparities and identify cost drivers, the Centers for Medicare & Medicaid Services (CMS) developed the Original Medicare Geographic Variation Public Use File (OM GV PUF).

This dataset is primarily compiled from CMS’s Chronic Conditions Data Warehouse (CCW), capturing 100% of Medicare claims for beneficiaries enrolled in the Original Medicare program, alongside demographic and eligibility data. Covering calendar years 2014–2024, the dataset tracks geographic location, demographic composition, spending indicators, and service utilization rates across national, state, and county levels.

- Dataset Source: CMS Geographic Variation Public Use File
- Project Objective: The primary goal of this project is to analyze and predict the Total Standardized Medicare Payments Per Capita across different geographic      regions. By building a high-performing predictive framework, this project aims to support healthcare policy forecasting, identify regional spend drivers, and      assist in strategic budget allocation.

## Data Structure Overview

The analysis leverages a highly structured multi-dimensional dataset spanning a decade of CMS records. The features are organized into logical thematic groups to systematically isolate demographic variables from clinical utilization metrics.

Target Variable: TOT_MDCR_STDZD_PYMT_PC (Total Standardized Medicare Payments Per Capita). This metric standardizes payments by removing geographic differences in payment rates (such as local wage indexes and graduate medical education adjustments), allowing for a true "apples-to-apples" comparison of spending across different regions.

Feature Classifications:

- Geographic & Demographic Data: Explores regional identifiers, age distributions, gender ratios, and dual-eligibility status.
- Spending Indicators: Captures broken-down payment metrics across distinct medical sectors (e.g., inpatient, outpatient, home health, hospice, and post-acute care).
- Service Utilization Metrics: Contains quantifiable operational counts, including admission rates, visit frequencies, and procedure volumes per capita.

Feature Engineering Strategy: Predictors were segmented into distinct feature groups to benchmark performance. Feature Group 2 was engineered as a curated subset combining demographic controls and structural spending indicators, while the Service Utilization Group focused heavily on healthcare delivery volume metrics.

## Executive Summary

This project establishes an optimized predictive framework to forecast regional Medicare spending behavior. By evaluating multiple feature subsets and regression frameworks, the analysis successfully demonstrates that a Linear Regression model utilizing Feature Group 2 yields exceptional accuracy in predicting TOT_MDCR_STDZD_PYMT_PC, as demonstrated by a robust $R^2$ value.A key technical challenge addressed during the modeling phase was the presence of severe multicollinearity within the Service Utilization feature block. While these overlapping dependencies introduce instability into individual feature weights—making causal explanation difficult—the final model was intentionally optimized as a pure predictive engine. The resulting architecture successfully maximizes variance explanation, providing healthcare administrators with a highly dependable tool for forecasting aggregate spending patterns across diverse geographic cohorts.

## Insights Deep Dive

1. Model Performance and Predictive PowerThe Linear Regression model trained on Feature Group 2 emerged as the top-performing framework for predicting regional standardized payments. The exceptionally high $R^2$ metric confirms that a substantial majority of the variance in regional Medicare spending can be accurately explained by a structured combination of demographic distributions and baseline utilization indicators.
   
2. The Multicollinearity Trade-off: Prediction vs. ExplanationA deep dive into the Service Utilization feature group revealed high multicollinearity. In standard Ordinary Least Squares (OLS) Linear Regression, highly correlated independent variables (e.g., highly correlated rates of diagnostic testing, inpatient stays, and specialized clinical visits) cause the algorithm's matrix calculations to become unstable. This results in highly erratic, volatile feature weights that fluctuate significantly with minor adjustments to the data.However, because the primary objective of this specific model iteration was strictly to maximize predictive accuracy rather than to isolate and interpret individual feature coefficients, this multicollinearity did not degrade the model’s overall forecasting capability. The model successfully maps the joint distribution of these features to output highly accurate per capita cost estimates.

## Structural Assumptions and Limitations

While the linear approach achieved high predictive metrics, the analysis uncovered core structural limitations inherent to OLS:

- Assumption of Linearity: The OLS model assumes a stable, constant, and linear relationship across variables. Consequently, it remains blind to potential non-linear variables or threshold effects (e.g., regions where a sudden spike in a specific chronic disease triggers an exponential, rather than linear, jump in spending).
  
- Interpretability Constraints: Due to the aforementioned multicollinearity within utilization metrics, using this specific model to inform direct policy interventions on a single service type could lead to misleading conclusions, despite the model's overall accuracy.

## Recommendations
Based on the modeling outcomes and data constraints, the following steps are recommended for future deployment and research extensions:

1. Transition to Regularized Regression for Feature Isolation: If policymakers require an explanatory model to determine exactly which specific medical services drive the highest incremental cost, the project should transition to Ridge (L2) or Lasso (L1) regression. These techniques introduce a regularization penalty that stabilizes feature weights in the presence of severe multicollinearity, allowing for reliable feature ranking.
   
2. Implement Tree-Based and Non-Linear Architectures: To address the limitations of linear assumptions and capture potential non-linear interactions, future iterations should evaluate ensemble machine learning models such as Random Forests or Gradient Boosting Machines (XGBoost/LightGBM). These architectures handle multicollinearity natively and can naturally map complex, non-linear relationships.
   
3. Deploy Targeted Resource Allocations via Outlier Tracking: Use the current high-accuracy model to establish a regional "spending baseline." Geographic regions whose actual spending significantly exceeds the model's predicted TOT_MDCR_STDZD_PYMT_PC should be flagged as structural outliers. Healthcare networks can target these specific clusters for preventative care programs or utilization audits to improve spending efficiency.
   
4. Incorporate Temporal and Macroeconomic Fixed Effects: Given that the dataset spans 2014 to 2024, introducing year-over-year fixed effects or time-series variables will capture how macroeconomic shifts, major health crises, or sweeping policy changes (e.g., the expansion of telehealth) impact spending trajectories over time.
