# Insurance Sales Performance and Predictive Analytics
An end-to-end Data Analytics project analyzing agency performance, commercial line growth, and regional written premiums across 1,623 independent agencies from 2005 to 2014.

---

## Business Case and Objectives
In regional insurance operations, setting realistic sales targets and managing risk profile distribution is heavily reliant on clear historical baselines. Relying on basic year-over-year growth averages often overlooks complex relationships between active policy volume, local market saturation, and historical loss ratios.

Project Goals:
* Identify Performance Metrics: Map key drivers influencing total written premium volume across 6 US states.
* Assess Risk and Retention Trends: Evaluate customer loyalty baselines alongside annual claims fluctuations.
* Build Predictive Baselines: Develop a regression framework to estimate written premium thresholds to assist in proactive resource allocation.

---

## Tools and Environment
* Data Processing and Machine Learning: Python 3.13 (Pandas, NumPy, Scikit-Learn)
* Time-Series Framework: Facebook Prophet
* Structured Query Layer: SQL (SQLite Engine)
* Business Intelligence: Power BI Desktop (Interactive Dashboarding)
* Dataset: 213,328 original records sourced from Kaggle (Insurance Agency Performance Dataset)

---

## Data Pipeline and Cleaning Process
The raw data contained structural inconsistencies, placeholder values, and incomplete time periods that required deliberate cleaning before any modeling could take place:

* Scope Realignment: Excluded the year 2015 due to incomplete monthly records, ensuring annual trends remained comparable.
* Negative Value Filtering: Removed records with written premiums less than or equal to 0 representing policy cancellations or reversals rather than active sales performance. This focused the dataset on 147,760 verified operational records.
* Sentinel Value Treatment: High-risk financial metrics contained 99999 placeholders indicating missing records. These were treated by replacing them with NaN values and carefully imputing column medians to maintain mathematical integrity without creating artificial outlier skew.
* Feature Engineering: 
  * Derived AGENCY_AGE from the operational tenure since agency appointment.
  * Operationalized PREMIUM_GROWTH_YOY (capped between -100% and +1000% to handle extreme volatility).
  * Applied a log-transformation (log1p) to the highly right-skewed target variable (WRTN_PREM_AMT) to establish a normal distribution for machine learning.

---

## Exploratory Data Analysis and Business Insights
A multi-tier analytical deep-dive revealed three core structural insights within the portfolio:

* The Shift to Commercial Lines: While Personal Lines premium volume traditionally made up the larger share of the portfolio, a structural pivot occurred over the decade. Commercial Lines premiums grew by 114%, scaling from $95.5M in 2005 to $204.4M in 2014.
* High Regional Concentration: Market penetration is highly skewed geographically. The state of Ohio represents 58% of the entire portfolio's premium volume ($2.45B), pointing out a clear saturation risk and identifying a strong need for expansion strategies in lower-penetration states like Michigan ($45.7M).
* Underwriting Health: The portfolio shows strong underlying customer loyalty with a consistent 88.75% average customer retention rate. However, a severe loss ratio spike occurred in 2012, which was normalized through successful underwriting corrections in 2013 and 2014.

---

## SQL Analysis (PostgreSQL/SQLite)
A local insurance.db database was built to model relational connections between performance components. Key analytical queries included:

```sql
-- Annual Portfolio Premium and New Business Summary
SELECT
    STAT_PROFILE_DATE_YEAR AS Year,
    COUNT(*) AS Total_Records,
    ROUND(SUM(WRTN_PREM_AMT), 2) AS Total_Written_Premium,
    ROUND(AVG(WRTN_PREM_AMT), 2) AS Avg_Premium_Per_Record,
    ROUND(SUM(NB_WRTN_PREM_AMT), 2) AS Total_New_Business_Premium
FROM insurance_sales
GROUP BY STAT_PROFILE_DATE_YEAR
ORDER BY STAT_PROFILE_DATE_YEAR;
``` 
Queries also extracted agency specific risk vectors, ranking the top 10 agencies by volume while auditing their corresponding loss and customer retention ratios.Predictive Modeling FrameworkThe clean data was split into an 80/20 train-test ratio (random_state=42) using 13 input features to test distinct algorithmic approaches:Performance MetricLinear Regression (Baseline)Random Forest Regressor (Log Scale)Mean Absolute Error (MAE)1.4700.505Root Mean Squared Error (RMSE)1.8300.858R-Squared Score (Variance Explained)0.3090.848Analytical EvaluationLinear Regression Baseline: Performed poorly with an R-Squared score of 0.309, confirming that financial indicators and macro insurance premiums scale in non-linear configurations.Random Forest Ensemble: Significantly outperformed the baseline, capturing 84.8% of the variance on log-transformed premium values.Feature Importance Findings: Feature extraction proved that period incurred losses (PRD_INCRD_LOSSES_AMT) held the highest predictive power with an importance score of 0.35, closely followed by new business premium (NB_WRTN_PREM_AMT) at 0.26.Time Series Forecasting: Facebook Prophet was trained on macro annual totals from 2005 to 2012 to project downstream boundaries. While a test window of 2 data points limited structural R-Squared evaluation on macro trends, the model accurately picked up the overall plateau pattern within its confidence interval bounds.Interactive Business Intelligence (Power BI)A cohesive, 3-page interactive dashboard was built to communicate these technical findings clearly to non-technical operational stakeholders:Sales Overview Dashboard: Houses high-level corporate KPIs ($4.01bn Total Premium, $455.64M New Business) alongside time slicers and top agency rank layouts to track growth velocity.Financial Health Analysis: Displays product line deviations between Commercial Lines and Personal Lines, tracks dual-axis retention gauges, and utilizes an estate treemap to visually isolate geographic risk distribution.Predictive Analytics View: Bridges data science with business strategy by plotting the Random Forest predicted-versus-actual scatter cluster directly alongside Prophet's future confidence intervals and explicit model performance error margins.

```
## 📁 Project Structure
insurance_sales_forecasting/
├── data/
│   ├── raw/               # Original dataset
│   └── processed/         # Cleaned data, train/test splits
├── powerbi/
│ └── insurance_sales_forecasting.pbix
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_preprocessing_modeling.ipynb
│   └── 03_sql_queries.ipynb
├── reports/               # All charts,diagrams, figures, and pdf
├── sql/
└── README.md
```
How to Run Locally
Clone the project:

Bash
git clone [https://github.com/maanajipriyanshu/insurance-sales-forecasting.git](https://github.com/maanajipriyanshu/insurance-sales-forecasting.git)
cd insurance-sales-forecasting
Install core environment dependencies:

Bash
pip install pandas numpy matplotlib seaborn scikit-learn prophet
Execution order: Run Jupyter Notebooks 01 through 03 sequentially to rebuild the local SQLite infrastructure and export prediction sets. Open the .pbix file in Power BI Desktop to inspect visual relationships.


## Sample Visualizations
![Annual Premium Trend](reports/fig_5_1_annual_premium_trend.png)
![Model Comparison](reports/fig_5_9_model_comparison.png)
![Feature Importance](reports/fig_5_10_feature_importance.png)


## Power BI Dashboards
### Sales Overview Dashboard

![Sales Overview Dashboard](reports/Sales_Overview.png)

### Financial Health Dashboard

![Financial Health Dashboard](reports/Financial_Health.png)

### Predictive Analytics Dashboard

![Predictive Analytics Dashboard](reports/Predictive_Analytics.png)


### Insurance Sales Forecasting Dashboard
![Insurance Sales Forecasting Dashboard](reports/insurance_sales_forecasting.pdf)

## 🔗 Portfolio
- **GitHub:** https://github.com/maanajipriyanshu
- **LinkedIn:** https://www.linkedin.com/in/maanapriyanshurajput/
- **Previous Project:** (https://github.com/maanajipriyanshu/india-job-market-analysis)
