# Model Equations
**Project:** Retail Chain Monthly Sales — Regression-Based Business Insights  


---

## Dummy Variable Approach (Task 3)

### Why Dummy Variables Are Needed

Regression models require numeric inputs. Two variables in the dataset — `region` (4 categories: East, North, South, West) and `store_type` (4 categories: Airport, High Street, Mall, Residential) — are categorical. They cannot be entered into the regression as text. Dummy variable encoding converts each category into a binary (0 or 1) indicator column.

### The Dummy Trap and Reference Category

If all categories are encoded, the columns become perfectly collinear — any one column can be derived from the others. This causes the **dummy variable trap** (perfect multicollinearity), which makes regression estimation impossible. The standard solution is to **drop one category per variable**, which becomes the "reference category." All other coefficients are then interpreted relative to that reference.

### Reference Categories Selected

| Variable | Reference Category | Reason for Selection |
|---|---|---|
| `region` | **East** | East has the largest number of observations (104 stores) making it the most stable baseline |
| `store_type` | **Airport** | Airport stores represent the highest baseline sales context; using them as reference allows all other types to be measured against the highest-performing format |

### Dummy Columns Created

**For `region` (reference = East):**

| Dummy Column | Value = 1 When | Value = 0 When |
|---|---|---|
| `region_North` | Store is in North region | Store is in any other region |
| `region_South` | Store is in South region | Store is in any other region |
| `region_West` | Store is in West region | Store is in any other region |

*(East region is represented when all three dummies = 0)*

**For `store_type` (reference = Airport):**

| Dummy Column | Value = 1 When | Value = 0 When |
|---|---|---|
| `store_High Street` | Store type is High Street | Store type is any other format |
| `store_Mall` | Store type is Mall | Store type is any other format |
| `store_Residential` | Store type is Residential | Store type is any other format |

*(Airport store type is represented when all three dummies = 0)*

---

## Simple Regression Equations (Task 8)

### Model 1 — footfall

**Statistical equation:**
```
monthly_sales = 446,410.58 + 35.6780 × footfall
```

**Business meaning:**  
A store with zero footfall is estimated to generate a baseline of ₹4,46,411 in monthly sales (the intercept represents fixed revenue components such as online orders or standing contracts). For every additional visitor who enters the store in a month, monthly sales are expected to increase by approximately **₹35.68**. This is a strong and statistically significant relationship.

| Element | Value | Interpretation |
|---|---|---|
| Intercept (β₀) | 4,46,410.58 | Estimated baseline monthly sales |
| Coefficient (β₁) | 35.6780 | Each additional store visitor → +₹35.68 sales |
| R-Squared | 0.7363 | Footfall alone explains 73.6% of monthly sales variation |
| P-value | < 0.0001 | Extremely statistically significant |

---

### Model 2 — marketing_spend

**Statistical equation:**
```
monthly_sales = 560,777.35 + 2.1296 × marketing_spend
```

**Business meaning:**  
The baseline monthly sales without any marketing spend is estimated at ₹5,60,777. For every additional ₹1 invested in marketing, monthly sales are expected to increase by **₹2.13**. While this is statistically significant, the R² of 16.7% indicates that marketing spend alone is a weak predictor of sales — its primary role is likely indirect, through driving footfall.

| Element | Value | Interpretation |
|---|---|---|
| Intercept (β₀) | 5,60,777.35 | Estimated baseline monthly sales |
| Coefficient (β₁) | 2.1296 | Each ₹1 in marketing spend → +₹2.13 in sales |
| R-Squared | 0.1672 | Marketing spend explains only 16.7% of sales variation |
| P-value | < 0.0001 | Statistically significant but low practical power |

---

### Model 3 — staff_count

**Statistical equation:**
```
monthly_sales = 400,551.47 + 16,984.03 × staff_count
```

**Business meaning:**  
A store with no staff is estimated at a baseline of ₹4,00,551 (a theoretical floor). Each additional staff member is associated with **₹16,984 higher monthly sales**. This strong relationship (R² = 65.2%) likely reflects that larger stores with more staff also serve more customers. Caution: this does not imply that simply adding staff to any store will generate this incremental revenue.

| Element | Value | Interpretation |
|---|---|---|
| Intercept (β₀) | 4,00,551.47 | Theoretical baseline sales |
| Coefficient (β₁) | 16,984.03 | Each additional staff member → +₹16,984 in monthly sales |
| R-Squared | 0.6523 | Staff count explains 65.2% of sales variation |
| P-value | < 0.0001 | Extremely statistically significant |

---

## Multiple Regression Equation — Final Model (Model 4)

### Full Statistical Equation

```
monthly_sales =
    126,369.77
  + 28.8291  × footfall
  + 1.1736   × marketing_spend
  + 3,003.33 × inventory_availability_pct
  + 2,790.49 × staff_count
  + 7,601.23 × region_North
  + 20,900.94 × region_South
  + 18,892.97 × region_West
  − 22,531.68 × store_High Street
  − 10,340.14 × store_Mall
  − 39,934.03 × store_Residential
```

### Coefficient Explanation — Business Language

| Variable | Coefficient | Business Meaning | Significant? |
|---|---|---|---|
| **Intercept** | ₹1,26,369.77 | The estimated baseline monthly sales for an Airport store in the East region with all numeric predictors at zero. | Yes (p=0.003) |
| **footfall** | +₹28.83 per visitor | After accounting for all other variables, each additional monthly visitor is associated with ₹28.83 more in sales. This is the single strongest driver. | Yes (p<0.001) |
| **marketing_spend** | +₹1.17 per ₹1 spent | Each rupee of marketing spend is associated with ₹1.17 more in monthly sales — a modest but positive return. | Yes (p<0.001) |
| **inventory_availability_pct** | +₹3,003 per 1% | For every 1 percentage point improvement in inventory availability, monthly sales rise by approximately ₹3,003. Keeping shelves stocked meaningfully lifts revenue. | Yes (p<0.001) |
| **staff_count** | +₹2,790 per person | Each additional staff member is associated with ₹2,790 more in monthly sales, after controlling for footfall and other variables. | Yes (p=0.027) |
| **region_North** | +₹7,601 vs East | North region stores earn approximately ₹7,601 more than East stores, holding all else equal. However, this is not statistically significant. | No (p=0.281) |
| **region_South** | +₹20,901 vs East | South region stores earn approximately ₹20,901 more than East stores. Statistically significant regional premium. | Yes (p=0.003) |
| **region_West** | +₹18,893 vs East | West region stores earn approximately ₹18,893 more than East stores. Statistically significant regional premium. | Yes (p=0.003) |
| **store_High Street** | −₹22,532 vs Airport | High Street stores earn approximately ₹22,532 less per month than Airport stores, all else equal. | Yes (p=0.016) |
| **store_Mall** | −₹10,340 vs Airport | Mall stores earn approximately ₹10,340 less per month than Airport stores. Not statistically significant. | No (p=0.286) |
| **store_Residential** | −₹39,934 vs Airport | Residential stores earn approximately ₹39,934 less per month than Airport stores — the largest format penalty. | Yes (p<0.001) |

---

## Final Model Selected

**Selected Model: Model 4 — Multiple Regression**

### Reason for Selection

| Criterion | Model 4 Performance |
|---|---|
| R-Squared | 0.8296 — highest of all four models |
| Adjusted R-Squared | 0.8240 — confirms additional variables genuinely improve fit |
| Business coverage | Covers 4 numerical levers + 2 categorical structures |
| Statistical quality | F-statistic = 150.4, p < 2.29e-112 — overall model is highly significant |
| Decision utility | Simultaneously answers: what to market, how to staff, how to stock, where to invest |

Model 4 was selected because it explains 83% of monthly sales variation while remaining interpretable and actionable. Each coefficient corresponds to a real business decision that leadership can evaluate and act upon. The simple models, while useful for illustrating individual relationships, do not provide sufficient explanatory power or multi-dimensional insight for strategic resource allocation decisions.

The coefficient reductions between simple models and the multiple model (e.g., footfall coefficient drops from 35.68 to 28.83; staff_count from 16,984 to 2,790) confirm that the multiple model correctly distributes explanatory credit across correlated variables rather than attributing all variation to a single factor.
