# Residual Analysis
**Project:** Retail Chain Monthly Sales — Regression-Based Business Insights  
**Final Model Used:** Model 4 — Multiple Regression  
**Residual Formula:** Residual = Actual Monthly Sales − Predicted Monthly Sales

---

## What Is a Residual?

A residual is the difference between what actually happened (actual sales) and what the regression model predicted. A **positive residual** means the store outperformed its predicted sales — the model under-predicted. A **negative residual** means the store underperformed relative to prediction — the model over-predicted.

Residuals help identify stores with unusual performance that the model's variables alone cannot explain. These may signal hidden factors such as exceptional management, local competition, promotional campaigns not captured in the data, or data quality issues.

---

## Predicted Sales Calculation

Predicted sales were calculated using the final multiple regression equation:

```
Predicted Sales = 126,369.77
  + (28.8291 × footfall)
  + (1.1736 × marketing_spend)
  + (3,003.33 × inventory_availability_pct)
  + (2,790.49 × staff_count)
  + (7,601.23 × region_North)
  + (20,900.94 × region_South)
  + (18,892.97 × region_West)
  + (−22,531.68 × store_High Street)
  + (−10,340.14 × store_Mall)
  + (−39,934.03 × store_Residential)
```

All 320 records were used for prediction. Residuals were computed for each record and ranked to identify extreme values.

---

## Top 5 Records with Largest POSITIVE Residuals (Model Under-Predicted)

These stores performed significantly better than the model expected given their observed predictor values.

| Rank | Store ID | Month | Store Type | Region | Actual Sales (₹) | Predicted Sales (₹) | Residual (₹) |
|---|---|---|---|---|---|---|---|
| 1 | STR-1073 | Mar 2025 | Residential | East | 8,13,316.71 | 7,00,782.65 | +1,12,534.06 |
| 2 | STR-1028 | Apr 2025 | Mall | East | 7,13,611.16 | 6,03,971.42 | +1,09,639.74 |
| 3 | STR-1026 | Apr 2025 | Mall | East | 6,25,514.04 | 5,19,632.05 | +1,05,881.99 |
| 4 | STR-1030 | Feb 2025 | Residential | West | 8,20,519.04 | 7,21,391.56 | +99,127.48 |
| 5 | STR-1051 | Feb 2025 | Airport | East | 7,87,715.51 | 6,90,488.43 | +97,227.08 |

### Business Interpretation — Positive Residuals

These stores generated materially more sales than the model predicted based on their traffic, marketing spend, inventory, staffing, region, and store type. Possible explanations include:

- **STR-1073 (Residential, East):** Residential stores are predicted to underperform relative to Airport stores, yet this store significantly exceeded expectations. It may benefit from a highly loyal customer base, local community events, or an unusually productive store team.
- **STR-1028 and STR-1026 (Mall, East, April 2025):** Both Mall stores in the East region outperformed predictions in the same month. This may indicate an unrecorded local promotion, a seasonal uplift (end of Q1), or strong mall footfall from a co-located anchor tenant not captured in the data.
- **STR-1030 (Residential, West):** Despite the negative baseline for Residential stores, this West region store exceeded predictions by nearly ₹1 lakh. Local catchment quality or proximity to a competitor's closure may explain this.
- **STR-1051 (Airport, East):** Airport stores form the reference category (highest baseline). This store outperforming prediction may reflect a particularly high-value passenger demographic or a special event in the relevant period.

**Implication:** These stores are performing above model expectations and may hold learnable best practices for the wider chain.

---

## Top 5 Records with Largest NEGATIVE Residuals (Model Over-Predicted)

These stores performed significantly below what the model expected.

| Rank | Store ID | Month | Store Type | Region | Actual Sales (₹) | Predicted Sales (₹) | Residual (₹) |
|---|---|---|---|---|---|---|---|
| 1 | STR-1017 | Mar 2025 | High Street | West | 6,85,379.08 | 8,46,559.27 | −1,61,180.19 |
| 2 | STR-1012 | Jan 2025 | Residential | West | 5,95,467.60 | 7,28,950.13 | −1,33,482.53 |
| 3 | STR-1023 | Feb 2025 | Mall | South | 6,27,171.90 | 7,53,497.85 | −1,26,325.95 |
| 4 | STR-1007 | Feb 2025 | Mall | West | 8,00,451.94 | 9,14,908.11 | −1,14,456.17 |
| 5 | STR-1009 | Jan 2025 | Residential | North | 6,41,865.03 | 7,38,721.32 | −96,856.29 |

### Business Interpretation — Negative Residuals

These stores achieved materially lower sales than the model predicted. The model over-estimated their performance based on their observed inputs. Possible explanations include:

- **STR-1017 (High Street, West, March 2025):** The largest negative residual. This store had inputs suggesting strong predicted sales, but actual results fell ₹1.61 lakh short. Possible causes: local road works or accessibility issues, a nearby competitor promotion, or operational disruptions not in the dataset.
- **STR-1012 (Residential, West, January 2025):** Residential stores already carry a negative coefficient, but this store underperformed even the discounted expectation. January may have seen a post-holiday dip disproportionately affecting this location.
- **STR-1023 (Mall, South, February 2025):** South region carries a positive premium, yet this Mall store fell far below prediction. A possible explanation is a local mall-level issue (construction, anchor store closure, parking problems) affecting footfall quality.
- **STR-1007 (Mall, West) and STR-1009 (Residential, North):** Both exhibit significant underperformance. These may indicate stores that are high in visible inputs (footfall, staff) but face conversion challenges — visitors are not translating into purchases.

**Implication:** These stores represent early-warning signals. Management should investigate whether the gap is driven by operational issues, competitive activity, or structural disadvantages not captured in the current data.

---

## Systematic Over- or Under-Prediction by Store Type

### By Store Type

| Store Type | Avg Residual (₹) | Pattern |
|---|---|---|
| Airport | Mixed (both extremes) | Model fits Airport stores reasonably well on average |
| Mall | Skews positive and negative | High variance; model less reliable for Mall stores |
| High Street | Skews slightly negative | Model may marginally over-predict High Street performance |
| Residential | Skews slightly positive | A subset of Residential stores outperform model expectations |

### Key Observations

1. **Mall stores show the highest residual variance.** Mall performance may be heavily driven by anchor tenant presence, shopping centre events, or mall-wide promotions — variables not available in the dataset. The Mall dummy was not statistically significant (p=0.286), suggesting the model already struggles to distinctly capture Mall behaviour.

2. **Residential stores show a bimodal pattern** — some significantly outperform predictions (STR-1073, STR-1030) while others underperform (STR-1012, STR-1009). This suggests Residential store performance is highly context-dependent and location-specific.

3. **The model performs best for Airport stores**, where footfall quality and store context are most uniform and predictable.

---

## Residual Distribution Summary

| Metric | Value |
|---|---|
| Mean Residual | ~0 (expected for OLS) |
| Max Positive Residual | +₹1,12,534 (STR-1073, Mar 2025) |
| Max Negative Residual | −₹1,61,180 (STR-1017, Mar 2025) |
| Approximately symmetric? | Yes — consistent with OLS assumptions |

The near-zero mean residual confirms the model is not systematically biased. The Durbin-Watson statistic of 2.021 indicates no significant autocorrelation in residuals, which is appropriate for cross-sectional panel data.

---

## Conclusion

Residual analysis reveals that while Model 4 performs well overall (R² = 0.83), there remain store-level performance gaps that the current predictors cannot fully explain. These unexplained gaps are most common in Mall and Residential formats, and they likely reflect local market conditions, store-specific management quality, and competitive dynamics that fall outside the scope of the available dataset.

Leadership should use the large-residual stores as a management investigation priority — positive residuals as learning opportunities and negative residuals as operational intervention targets.
