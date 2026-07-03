# Geographical Medicare Analysis: Predicting Regional Per Capita Spending

## Background and Overview

Federal policymakers, health economists, and medical researchers have long recognized that the volume, cost, and quality of healthcare services received by Medicare beneficiaries vary substantially across different geographic regions of the United States. To understand these regional disparities and identify cost drivers, the Centers for Medicare & Medicaid Services (CMS) developed the Original Medicare Geographic Variation Public Use File (OM GV PUF).

This dataset is primarily compiled from CMS’s Chronic Conditions Data Warehouse (CCW), capturing 100% of Medicare claims for beneficiaries enrolled in the Original Medicare program, alongside demographic and eligibility data. Covering calendar years 2014–2024, the dataset tracks geographic location, demographic composition, spending indicators, and service utilization rates across national, state, and county levels.

- Dataset Source: [CMS Geographic Variation Public Use File](https://data.cms.gov/summary-statistics-on-use-and-payments/medicare-geographic-comparisons/medicare-geographic-variation-by-national-state-county/data)
- Project Objective: The primary goal of this project is to analyze and predict the Total Standardized Medicare Payments Per Capita (`TOT_MDCR_STDZD_PYMT_PC`) across different geographic regions. By building a high-performing predictive framework, this project aims to support healthcare policy forecasting, identify regional spend drivers, and assist in strategic budget allocation.

## Data Structure Overview

The analysis leverages a highly structured multi-dimensional dataset spanning a decade of CMS records. The features are organized into logical thematic groups to systematically isolate demographic variables from clinical utilization metrics.

Target Variable: `TOT_MDCR_STDZD_PYMT_PC` (Total Standardized Medicare Payments Per Capita). This metric standardizes payments by removing geographic differences in payment rates (such as local wage indexes and graduate medical education adjustments), allowing for an accurate baseline comparison of spending across regions.

Feature Classifications:
- Patient Demographics (Feature Group 1): Captures regional patient attributes, including average age (`BENE_AVG_AGE`), gender ratios (`BENE_FEML_PCT`), and Medicaid dual-eligibility status (`BENE_DUAL_PCT`).
- System Utilization (Feature Group 2): Explores granular per capita payment indicators across specific medical sectors (e.g., Inpatient `IP_MDCR_STDZD_PYMT_PC`, `Outpatient OP_MDCR_STDZD_PYMT_PC`, Home Health    `HH_MDCR_STDZD_PYMT_PC`, and Hospice `HOSPC_MDCR_STDZD_PYMT_PC`).
- Operational Baseline (Group 3): Maintained strictly as an un-engineered, aggregate baseline block to benchmark the comparative performance of our primary feature groups.

Modeling Strategy (Comparative Evaluation): Feature groups were tested head-to-head across three distinct mathematical frameworks to determine which approach best models regional spending:

- Linear Regression (Baseline Benchmark): Tests if a rigid, straight-line relationship can sufficiently capture regional cost distributions.
- Polynomial Regression (Non-Linear Challenger): Captures multi-sector interaction and geometric curves to see if non-linear flexibility yields a superior fit.
- Ridge Regression (Regularized Challenger): Introduces an $L2$ penalty to evaluate how stabilizing model weights handles severe feature dependencies compared to standard Linear Regression.

## Executive Summary

This project establishes an optimized predictive framework to forecast regional Medicare spending behavior. By evaluating multiple feature configurations across linear, regularized, and non-linear regression variations, the analysis successfully demonstrates that capturing interaction effects yields superior results.

The Polynomial Regression model utilizing Feature Group 2 emerged as the optimal analytical framework, achieving an exceptional $R^2$ score of 79.39%. This significantly outpaced demographic constraints and baseline linear configurations, proving that systemic sector utilization costs map the vast majority of geographic payment variance when non-linear expansions are captured.

A key technical hurdle addressed during the modeling phase was managing severe multicollinearity within the structural sector spending features. While these overlapping dependencies introduce instability into standard linear feature weights (making causal, independent explanation difficult), modeling the collaborative interaction terms via a polynomial expansion successfully utilizes these joint relationships to optimize aggregate predictive performance.

## Insights Deep Dive

1. Model Performance and Non-Linear Supremacy Comparative evaluation across the true feature blocks demonstrated that a purely linear assumption underrepresents the true structural cost dynamics. Capturing non-linear interactions and polynomial expansions resulted in a distinct performance curve: Feature Group 1 (Demographics Only): Achieved a very low polynomial baseline ($R^2 \approx 11.32\%$), proving that demographics alone cannot reliably explain or forecast total regional healthcare expenditures. Feature Group 2 (System Utilization & Costs): Reached a peak predictive accuracy of $R^2 \approx 79.43\%$ when expanded to capture non-linear interactions. This highlights that sector expenses compound non-linearly across geographies.

2. The Multicollinearity Trade-off: Prediction vs. Explanation. A deep dive into the sector spending metrics within Feature Group 2 revealed high levels of multicollinearity. In standard linear regression models, these overlapping dependencies create highly erratic, volatile feature weights, which can distort the algorithm when trying to isolate independent variables. However, because the primary goal of this specific modeling track was strictly to maximize predictive accuracy rather than to isolate clean, uncorrelated individual feature weights, this multicollinearity does not compromise the model's aggregate forecasting power. By leaning into a polynomial space, the framework maps these correlated clusters to capture combined real-world spending surges.

3. Degrees of Freedom and Complexity Trade-offs: Transitioning to a polynomial model increases the feature space's complexity and alters the structural degrees of freedom. While the expansion captures critical interaction terms—such as how a concurrent rise in both Home Health and Inpatient service utilization compounds overall per capita spending—it demands strict validation to ensure that the higher degrees of freedom do not introduce localized overfitting.

## Structural Assumptions and Limitations

While the polynomial approach achieved high predictive metrics, the analysis uncovered core structural limitations:

1. Part-to-Whole Inflation: Feature Group 2 consists of component healthcare sector costs (Inpatient, Outpatient, etc.) used to predict the total overall spending. This structural relationship heavily guides the model toward high performance, which may obscure real-world variations.
   
2. Explanatory Collapse: As previously mentioned, the independent variables within the cost groups exhibit severe multicollinearity. While expanding the feature space into a polynomial structure successfully captures complex interaction terms to maximize predictive accuracy, it disrupts the stability of individual feature weights. This limits the model's utility as an explanatory tool for targeted policy interventions.
   
3. Complexity & Omitted Factors: Moving to a polynomial feature matrix significantly increases model complexity and degrees of freedom. Furthermore, the model remains blind to vital external cost drivers such as regional chronic disease tracking, local hospital bed capacities, and community Social Determinants of Health (SDoH). Future iterations should focus on incorporating these external clinical indicators.

## Recommendations

Based on the modeling outcomes, the following steps are recommended for portfolio and operational extensions:

- Leverage the High-Accuracy Baseline for Regional Budgeting: Deploy the Feature Group 2 Polynomial Regression model as the primary forecasting tool for regional Medicare cost projections. Because it captures interaction metrics, it serves as a highly reliable tool for predicting baseline spending trajectories.
  
- Transition to Ridge/Lasso Regularization for Policy Explanation: When policymakers require an explanatory model to determine exactly which specific service line drives cost increases, use regularized regression. The implemented Ridge Regression framework introduces a penalty that stabilizes the coefficients in the presence of multicollinearity, ensuring interpretable feature weights.
  
- Incorporate Tree-Based Algorithms: To naturally handle multicollinearity and scale beyond the degrees-of-freedom constraints of polynomial feature matrices, future developments should implement XGBoost or Random Forests. These architectures evaluate complex, non-linear feature splits natively without expanding the underlying column matrix manually.
  
- Identify Care-Management Anomalies: Use the polynomial model's predictions to track geographic regions whose actual spending significantly exceeds the model's outputs. These regions represent highly anomalous spending spikes that cannot be explained by standard demographic and service interactions, serving as ideal targets for clinical audits or preventative healthcare interventions.
