# 📉 Customer Churn & Retention Analysis

A Python and SQL project that analyzes **700 customers** across customer, subscription, and support data to find **churn patterns**, calculate key retention KPIs, and segment customers by **churn risk**. It uses Pandas for analysis, SQLite for data storage, and Matplotlib/Seaborn for visualization.

---

## 📌 Project Overview

**Purpose:** To understand why customers leave a subscription service, measure how much revenue is exposed, and identify which customers need closer retention attention.

**Key questions answered:**
- What are the overall churn and retention rates?
- Which plans, contract types, and states have the highest churn?
- Why do customers cancel?
- How much monthly revenue is at risk from churned customers?
- Do support escalations relate to churn?
- Which customers fall into low, medium, and high churn-risk groups?

---

## 🛠️ Tech Stack

| Category | Tools |
|---|---|
| Language | Python |
| Data Analysis | Pandas, NumPy |
| Visualization | Matplotlib, Seaborn |
| Database | SQLite (`sqlite3`) |
| Data Source Format | Excel (`.xlsx`) |
| Environment | Jupyter Notebook |

---

## 📂 Data Source

The raw data is provided as an Excel file (`customer_churn_data_raw.xlsx`) with 3 sheets, which the notebook loads into a SQLite database (`customer_churn.db`) as 3 tables.

| Table | Records | Columns |
|---|---|---|
| **db_customer** | 700 | customerid, name, country, state, gender, dob, interests, pincode |
| **db_subscription** | 700 | customerid, subscription_start_date, subscription_type, renewal_date, plan_type, contract_type, cancellation_date, cancellation_reason, monthly_charges, cltv, churn_score |
| **db_support** | 300 | customerid, complaint_date, escalations, csat_score, col_1, comment |

- **Customers:** 700 (India and Nepal)
- **Support records:** 300 complaints from 247 customers
- **Plans:** Basic, Standard, Premium
- **Contracts:** Monthly, Annual

---

## ⚙️ Project Workflow

1. **Data Import:** Loaded the SQLite tables into Pandas DataFrames using SQL queries (or created the database from the Excel file).
2. **Data Cleaning:**
   - Renamed columns (`name` → `customer_name`)
   - Dropped unused columns (`interests`, `pincode`, `col_1`, `comment`)
   - Converted date columns to datetime
   - Standardized gender values (Men/Women → Male/Female)
   - Filled missing `country` values by mapping them from `state`
3. **Feature Engineering:**
   - `churn_flag` (1 if a cancellation date exists)
   - `tenure_days` (cancellation date or today, minus subscription start date)
   - `complaint_count` per customer
   - `churn_risk` (Low < 50, Medium 50–69, High ≥ 70, based on churn score)
4. **Data Merging:** Handled duplicate support records (keeping each customer's latest complaint), then joined the three tables on `customerid`, checking the data shape before and after each join.
5. **KPI Analysis:** Calculated churn rate, retention rate, ARPU, revenue at risk, escalation rate, and more.
6. **Visualization:** Built charts with Matplotlib and Seaborn.
7. **Export:** Saved the final merged dataset as `exported_churn_data.csv`.

---

## 📊 KPIs Calculated

| KPI | Result |
|---|---|
| Total Customers | **700** |
| Churned Customers | **170** |
| Churn Rate | **24.29%** |
| Retention Rate | **75.71%** |
| ARPU (average monthly charges) | **26.85** |
| Revenue at Risk (monthly charges of churned customers) | **4,561.30** (about 24% of total) |
| Escalation Rate | **7.57%** |
| Average Complaints per User | **0.43** |
| Correlation: Escalations vs Churn | **0.01** |

**Churn by plan type**

| Plan | Customers | Churn Rate | Total Monthly Charges |
|---|---|---|---|
| Basic | 167 | **28.14%** | 1,349.33 |
| Standard | 256 | 24.61% | 3,894.44 |
| Premium | 277 | 21.66% | 13,548.23 |

**Churn by contract type**

| Contract | Customers | Churn Rate |
|---|---|---|
| Monthly | 302 | **26.16%** |
| Annual | 398 | 22.86% |

**Cancellation reasons (170 churned customers)**

| Reason | Customers |
|---|---|
| Switched to competitor | 45 |
| Forgot to cancel trial | 38 |
| Not enough content | 33 |
| Too expensive | 30 |
| Poor streaming quality | 24 |

---

## 📈 Visualizations

- Monthly churn trend (line chart)
- Churn rate by plan type (bar chart)
- Churn rate by state (bar chart)
- Correlation heatmap (Seaborn and Matplotlib)
- Pairplot of the encoded features
- Catplot comparing monthly charges by plan, gender, and churn risk
- Pivot tables for churn by plan type

### Chart Previews
![Churn by Plan](https://github.com/KaranShinde016/Customer-Churn-Analysis-Project/blob/main/churn_by_plan.png)
![Churn by State](https://github.com/KaranShinde016/Customer-Churn-Analysis-Project/blob/main/churn_by_state.png)
![Correlation Heatmap](https://github.com/KaranShinde016/Customer-Churn-Analysis-Project/blob/main/correlation_heatmap.png)

---

## 🔍 Key Insights

- **24.29%** of customers (170 of 700) churned, so retention is **75.71%**.
- The **Basic plan has the highest churn (28.14%)** and Premium the lowest (21.66%).
- **Premium** customers generate about **72%** of total monthly charges (13,548 of 18,792), so keeping them is the most important for revenue.
- **Monthly contracts churn more (26.16%)** than annual contracts (22.86%).
- The top cancellation reason is **switching to a competitor** (45 customers), followed by forgetting to cancel a trial (38) and not enough content (33).
- **Rajasthan (34.7%)** and **Karnataka (32.4%)** have the highest state-level churn.
- Churned customers represent **4,561.30** in monthly charges, about **24%** of the total.
- Support escalations show almost no correlation with churn (0.01), so complaints alone do not explain why customers leave in this data.
- Churn risk segments: **365 low, 165 medium, and 170 high**. Every customer with a churn score of 70 or more has already cancelled.

---

## 🌟 Highlights

- Combined **SQL (SQLite) and Python** in one workflow
- Cleaned and standardized data from 3 related tables (700 customers)
- Engineered features for churn, tenure, complaints, and risk segments
- Calculated 9+ business KPIs
- Used correct ordered encoding (Basic < Standard < Premium) for the correlation heatmap

---

## 📁 Repository Structure

```
Customer-Churn-Analysis/
│
├── churn_analysis.ipynb            # Main analysis notebook
├── customer_churn.db               # SQLite database
├── customer_churn_data_raw.xlsx    # Raw data (Excel)
├── exported_churn_data.csv         # Cleaned and merged output
└── README.md
```

---

## 🚀 How to Run

1. Clone or download this repository.
2. Install the required libraries:
   ```bash
   pip install pandas numpy matplotlib seaborn openpyxl jupyter
   ```
3. Open `churn_analysis.ipynb` in Jupyter Notebook.
4. Run the cells in order. If the `.db` file gives any problem, run the notebook cell that rebuilds it from the Excel file.

---

## 🙏 Credits

This project is based on a guided **Churn Analysis and Customer Intelligence** walkthrough by **Rishabh Mishra**, and the dataset comes from that guided project.

---

## 👤 Author

**Karan Shinde**
Data Analyst | SQL | Python | Power BI | Tableau | Excel

📧 karanshinde1608@gmail.com
🔗 [LinkedIn](https://www.linkedin.com/in/karan-shinde-56a35529a)
