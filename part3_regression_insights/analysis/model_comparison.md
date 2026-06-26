# Model Comparison Analysis
**Project:** Retail Chain Monthly Sales — Regression-Based Business Insights  

**Dataset:** business_regression_data.xlsx | 320 observations × 14 variables

---

## Overview

Four regression models were estimated with `monthly_sales` as the dependent variable. Three simple linear regression models were run first to evaluate individual predictors; a final multiple regression model was then built combining the strongest predictors along with dummy variables for store type and region.

---

## Model 1 — Simple Regression: footfall

| Item | Detail |
|---|---|
| **Model Name** | Model 1 – Simple Linear Regression (footfall) |
| **Dependent Variable** | monthly_sales |
| **Independent Variable** | footfall |
| **R-Squared** | 0.7363 |
| **Adjusted R-Squared** | 0.7352 |
| **Coefficient** | 35.6780 |
| **Intercept** | 446,410.58 |
| **P-value (coefficient)** | < 0.0001 |
| **F-statistic** | 887.52 |
| **Significant Variable** | footfall ✓ |

### Business Usefulness
**HIGH.** Footfall alone explains 73.6% of monthly sales variation — the strongest single predictor identified. This confirms that in-store traffic volume is a primary revenue driver. Leadership should prioritise strategies that increase store footfall (location, promotions, events).

### Limitations
- Ignores marketing investment, staffing levels, inventory, and store-type effects.
- Footfall itself may be influenced by marketing spend, creating an indirect effect chain.
- Cannot distinguish between high-spend versus low-spend visitors.

---

## Model 2 — Simple Regression: marketing_spend

| Item | Detail |
|---|---|
| **Model Name** | Model 2 – Simple Linear Regression (marketing_spend) |
| **Dependent Variable** | monthly_sales |
| **Independent Variable** | marketing_spend |
| **R-Squared** | 0.1672 |
| **Adjusted R-Squared** | 0.1646 |
| **Coefficient** | 2.1296 |
| **Intercept** | 560,777.35 |
| **P-value (coefficient)** | < 0.0001 |
| **F-statistic** | 63.74 |
| **Significant Variable** | marketing_spend ✓ |

### Business Usefulness
**MODERATE.** While statistically significant, marketing_spend alone explains only 16.7% of sales variance. The low R² suggests marketing drives sales indirectly — likely by boosting footfall — rather than directly. Marketing spend should not be evaluated in isolation from traffic outcomes.

### Limitations
- Very low explanatory power as a standalone model.
- Likely mediating effect: marketing → footfall → sales.
- High variation in sales unexplained by marketing alone.

---

## Model 3 — Simple Regression: staff_count

| Item | Detail |
|---|---|
| **Model Name** | Model 3 – Simple Linear Regression (staff_count) |
| **Dependent Variable** | monthly_sales |
| **Independent Variable** | staff_count |
| **R-Squared** | 0.6523 |
| **Adjusted R-Squared** | 0.6511 |
| **Coefficient** | 16,984.03 |
| **Intercept** | 400,551.47 |
| **P-value (coefficient)** | < 0.0001 |
| **F-statistic** | 596.25 |
| **Significant Variable** | staff_count ✓ |

### Business Usefulness
**HIGH.** Staff count explains 65.2% of sales variance. However, this likely reflects store size and capacity rather than a pure staffing effect. Larger stores have both more staff and higher sales. Leadership should interpret this as a store-scale indicator rather than evidence that simply hiring more staff increases sales.

### Limitations
- Possible confound: staff_count proxies store size, not productivity.
- Hiring more staff at small stores may not replicate the sales effect.
- Does not account for skill, experience, or role mix.

---

## Model 4 — Multiple Regression (FINAL MODEL ★)

| Item | Detail |
|---|---|
| **Model Name** | Model 4 – Multiple Regression (Final) |
| **Dependent Variable** | monthly_sales |
| **Independent Variables (Numerical)** | footfall, marketing_spend, inventory_availability_pct, staff_count |
| **Independent Variables (Dummy)** | region_North, region_South, region_West (ref: East); store_High Street, store_Mall, store_Residential (ref: Airport) |
| **R-Squared** | 0.8296 |
| **Adjusted R-Squared** | 0.8240 |
| **F-Statistic** | 150.4 |
| **Prob(F-statistic)** | 2.29e-112 |
| **Number of Predictors** | 10 |

### Significant Variables (p < 0.05)

| Variable | Coefficient | P-value | Direction |
|---|---|---|---|
| footfall | 28.8291 | < 0.0001 | Positive ↑ |
| marketing_spend | 1.1736 | < 0.0001 | Positive ↑ |
| inventory_availability_pct | 3,003.33 | < 0.0001 | Positive ↑ |
| staff_count | 2,790.49 | 0.0269 | Positive ↑ |
| region_South | 20,900.94 | 0.0035 | Positive ↑ |
| region_West | 18,892.97 | 0.0028 | Positive ↑ |
| store_High Street | -22,531.68 | 0.0160 | Negative ↓ |
| store_Residential | -39,934.03 | < 0.0001 | Negative ↓ |

### Variables Not Statistically Significant (p ≥ 0.05)

| Variable | Coefficient | P-value | Note |
|---|---|---|---|
| region_North | 7,601.23 | 0.2806 | Not significant vs East |
| store_Mall | -10,340.14 | 0.2859 | Not significant vs Airport |

### Business Usefulness
**HIGHEST.** Model 4 explains 82.96% of monthly sales variance — a substantial improvement over all simple models. It simultaneously captures the effect of store traffic, marketing investment, inventory readiness, staffing, geographic region, and store format. This makes it the most actionable model for leadership decision-making across multiple business levers.

### Limitations
- Some potential multicollinearity between footfall and staff_count (both correlate with store size).
- holiday_flag and customer_rating were excluded (low correlation); their effect may be non-linear.
- avg_discount_pct and competitor_distance_km showed weak or negative correlations and were not included.
- Regression shows association, not causation — observational data cannot confirm that changing one variable will produce the predicted sales change.
- The model does not capture seasonal trends, local economic conditions, or store age.

---

## Comparative Summary Table

| Model | R-Squared | Adj R-Squared | Predictors | Most Useful For | Recommended? |
|---|---|---|---|---|---|
| Model 1 (footfall) | 0.7363 | 0.7352 | 1 | Quick traffic-sales estimate | Supplementary |
| Model 2 (marketing_spend) | 0.1672 | 0.1646 | 1 | ROI awareness only | Limited |
| Model 3 (staff_count) | 0.6523 | 0.6511 | 1 | Store-size proxy | Supplementary |
| **Model 4 (Multiple — FINAL)** | **0.8296** | **0.8240** | **10** | **Strategic decisions** | **YES ★** |

---

## Conclusion

Model 4 (Multiple Regression) is the recommended final model. It offers the highest explanatory power, covers all key business levers (traffic, marketing, inventory, staffing, region, store format), and produces actionable insights for each dimension of the retail chain's operations.

#### Key Regression Results
- Model 4 (Final): R² = 0.8296 — explains 83% of monthly sales variation
- Top drivers: footfall (+₹28.83/visitor), inventory availability (+₹3,003/1%), marketing spend (+₹1.17/₹)
- Format penalty: Residential stores earn ₹39,934 less/month vs Airport stores
- Reference categories: East (region), Airport (store type)
