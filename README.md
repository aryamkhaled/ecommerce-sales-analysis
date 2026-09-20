# E-Commerce Sales \& Customer Intelligence 🛍️

## Project Overview

Dataset: Online Retail Dataset (UCI Machine Learning Repository)

Goal: Analyze sales patterns, customer behavior, and product performance to generate actionable business insights.

Author: **Aryam Khaled**

Date: **September 2026**

Repository: \[Your GitHub Link]

\---

## Libraries Used

- **python**
- **Data manipulation**

import pandas as pd

import numpy as np

- **Data visualization**

import matplotlib.pyplot as plt

import seaborn as sns

- **Date handling**

from datetime import datetime

- **Set visualization style**

sns.set_style('whitegrid')

plt.rcParams\['figure.figsize'] = (12, 6)

%matplotlib inline

\---

## 📊 1. Data Loading \& Initial Inspection

### 1.1 Load Dataset

python

\# Load the Excel file

df_Ret = pd.read_excel('Online_Retail.xlsx')

### 1.2 Initial Data Inspection

python

\# Display first 5 rows

df_Ret.head()

**_Output:_** Shows first 5 transactions with 8 columns.

\---

## 🔍 2. Data Understanding

### 2.1 Dataset Structure

python

\# Check dataset size

df_Ret.shape

**_Result:_** **(541909, 8) → 541,909 transactions and 8 columns.**

### 2.2 Column Descriptions

Column Name Description Data Type Example

- **InvoiceNo** Unique invoice/order identifier object (string) "536365"
- **StockCode** Product code (internal SKU) object (string) "85123A"
- **Description** Product name/description object (string) "WHITE HANGING HEART T-LIGHT HOLDER"
- **Quantity** Number of units purchased int64 6
- **InvoiceDate** Transaction date and time datetime64 "2010-12-01 08:26:00"
- **UnitPrice** Price per unit (in GBP) float64 2.55
- **CustomerID** Unique customer identifier float64 17850.0
- **Country** Customer's shipping country object (string) "United Kingdom"

### 2.3 Data Types \& Memory Usage

python

\# Display data types and non-null counts

df_Ret.info()

**_Result_**:

· **541,909 entries**

· **8 columns**

· InvoiceDate correctly parsed as datetime

· CustomerID has missing values

### 2.4 Missing Values

python

\# Check for missing values in each column

df_Ret.isnull().sum()

**_Result:_**

· CustomerID: **135,080 missing (24.9%)**

· Description: **1,454 missing (0.3%)**

### 2.5 Unique Values Count

python

\# Count unique values in each column

df_Ret.nunique()

**_Result_**:

· **25,900** unique invoices

· **4,070** unique products

· **4,372** unique customers

· **38** unique countries

### 2.6 Statistical Summary (Numerical Columns)

python

\# Summary statistics for numerical columns

df_Ret.describe()

**_Observations_**:

· Quantity ranges from -80,995 to 80,995 (contains returns)

· UnitPrice ranges from -11,062 to 38,970 (has outliers)

### 2.7 Statistical Summary (Text Columns)

python

\# Summary statistics for text columns

df_Ret.describe(include=\['object','str'])

**_Observations_**<i>:</i>

· Most transactions from United Kingdom

· Multiple product descriptions exist

### 2.8 Random Sample Check

python

\# Random sample of 10 rows to check data diversity

df_Ret.sample(10)

**_Purpose_**: Ensures data diversity and consistency across all rows.

###### &#x20;✅ _Data Understanding Phase Complete_

All initial data exploration and quality checks have been completed.

The dataset is now well-understood and ready for the cleaning phase.

\---

## 🧹 3. Data Cleaning

#### &#x20;Cleaning Steps Performed

##### 1\. Removed rows with missing descriptions

&#x20; - Rows dropped: **1,454**

&#x20; - **_Reason_**: Product information is essential for analysis

##### 2\. Removed returns, invalid prices, cancelled invoices, and duplicates

&#x20; - Total rows dropped: **15,577**

&#x20; - **_Reason_**: To keep only clean, valid sales transactions

#####

- ##### &#x20;New Feature Created

&#x20; - **_TotalSales = Quantity × UnitPrice_**

&#x20; - Provides total revenue per transaction row

&#x20; - Enables revenue-based analysis

- ##### &#x20;Final Dataset Size

#####

&#x20; - _Original rows:_ **541,909**

&#x20; - _Final rows_: **524,878**

&#x20; - _Columns:_ **9** (8 original + TotalSales)

#####

- ##### &#x20;Key Improvements

&#x20; - Clean data is the foundation of trustworthy analysis.

&#x20; - Cleaned dataset now contains only valid sales transactions

&#x20; (**_Reduction_**<i>:</i> **17,031 / 541,909 × 100 ≈ 3.1%**)

&#x20; - All rows have complete descriptions and valid prices

&#x20; - Returns and cancellations are excluded

&#x20; - Revenue column added for deeper analysis

\---

## ⚙️ 4. Feature Engineering

#### &#x20;4.1 Date-Based Features

**_Extracted from `InvoiceDate` column to enable time-based analysis:_**

&#x20; - **Month**: To analyze seasonal patterns (_1-12_)

&#x20; - **DayOfWeek**: To identify peak shopping days (_0 = Monday, 6 = Sunday_)

&#x20; - **Hour**: To find preferred shopping hours (_0-23_)

&#x20; - **Quarter**: For quarterly performance analysis (_1-4_)

&#x20; - **IsWeekend**: Binary flag for weekend transactions (_1 = weekend, 0 = weekday_)

**_Result_**: 5 new columns added. Shape changed from **(524,878, 9) to (524,878, 14)**.

#### &#x20; 4.2 Customer-Based Features

**_Aggregated data per customer to build a customer profile:_**

&#x20; - **FirstPurchase**: Date of first transaction

&#x20; - **LastPurchase**: Date of last transaction

&#x20; - **NumOrders**: Total number of orders per customer

&#x20; - **TotalSpent**: Total revenue generated by each customer

&#x20; - **LoyaltyDays**: Days between first and last purchase

&#x20; - **AvgOrderValue**: Average spending per order

**_Result_**: Created a customer-level dataset with **4,338 unique customers and 7 columns**.

#### &#x20;4.3 Customer Segmentation

**_Classified customers into three tiers based on total spending:_**

&#x20; - **Gold**: £10,000+ spent → 104 customers (**2.40%**)

&#x20; - **Silver**: £3,000 - £9,999 spent → 450 customers (**10.37%**)

&#x20; - **Bronze**: Less than £3,000 spent → 3,784 customers (**87.23%**)

###### &#x20;Final Dataset

&#x20; - Rows: **524,878**

&#x20; - Columns: **14 (9 original + 5 date features)**

###### &#x20;Customer Features Dataset

&#x20; - Rows: **4,338**

&#x20; - Columns: **_8_ (CustomerID, FirstPurchase, LastPurchase, NumOrders, TotalSpent, LoyaltyDays, AvgOrderValue, Segment)**

##### &#x20; ⭐️Key Insights from Feature Engineering

**1. Customer Concentration:**

&#x20; - Only **2.40%** of customers are **"Gold"** tier, but they likely generate a significant portion of revenue.

**2. Loyalty Patterns:**

&#x20; - Some customers have **`LoyaltyDays = 0`**, meaning they made all their **purchases on the same day**.

**3. Order Frequency:**

&#x20; - The customer with **`NumOrders = 182`** is a **highly** engaged repeat customer.

**4. Average Order Value:**

&#x20; - Ranges from **_very low values_** (_e.g._, **£19.67**) **_to extremely high values_** (_e.g._, **£77,183.60**).

\---

## 📈 5. Analysis \& Visualization

### 5.1 Sales Analysis

#### 5.1.1 Top Products by Quantity

**Top 5 Best-Selling Products:**

1. PAPER CRAFT, LITTLE BIRDIE — 80,995 units
2. MEDIUM CERAMIC TOP STORAGE JAR — 78,033 units
3. WORLD WAR 2 GLIDERS ASSTD DESIGNS — 54,951 units
4. JUMBO BAG RED RETROSPOT — 48,371 units
5. WHITE HANGING HEART T-LIGHT HOLDER — 37,872 units

#### 5.1.2 Top Products by Revenue

**After excluding non-products:**

1. REGENCY CAKESTAND 3 TIER — £174,156.54
2. PAPER CRAFT, LITTLE BIRDIE — £168,469.60
3. WHITE HANGING HEART T-LIGHT HOLDER — £106,236.72
4. PARTY BUNTING — £99,445.23
5. JUMBO BAG RED RETROSPOT — £94,159.81

_Note: Non-product entries (DOTCOM POSTAGE, POSTAGE, Manual) were excluded for accurate ranking._

#### 5.1.3 Top Customers

**Top 5 by Total Spent:**

1. Customer 14646 — £280,206.02 (2,076 orders)
2. Customer 18102 — £259,657.30 (431 orders)
3. Customer 17450 — £194,390.79 (336 orders)
4. Customer 16446 — £168,472.50 (3 orders)
5. Customer 14911 — £143,711.17 (5,670 orders)

_All top 10 customers belong to the "Gold" segment._

---

### 5.2 Time Analysis

#### 5.2.1 Monthly Sales Trend

- **Peak Month:** November (£1,453,295)
- **Second Peak:** December (£1,391,044)
- **Lowest Month:** February (£508,081)

_Note: Month 12 combines data from December 2010 and December 2011._

#### 5.2.2 Sales by Day of Week

| Day       | Revenue     |
| --------- | ----------- |
| Thursday  | £2,133,746  |
| Tuesday   | £2,086,320  |
| Wednesday | £1,783,645  |
| Friday    | £1,768,302  |
| Monday    | £1,684,294  |
| Sunday    | £798,811    |
| Saturday  | £0 (closed) |

#### 5.2.3 Sales by Hour

- **Peak Hour:** 12 PM (£1,414,006)
- **Second Peak:** 10 AM (£1,402,866)
- **Peak Window:** 10 AM – 3 PM
- **Closed:** Before 7 AM and after 8 PM

#### 5.2.4 Sales Heatmap (Day × Hour)

- **Hotspot:** Tuesday–Thursday × 10 AM – 3 PM

#### 5.2.5 Weekend vs Weekday

- **Weekday:** 92.21% (£9,456,309)
- **Weekend:** 7.79% (£798,811)

---

### 5.3 Product Analysis

#### 5.3.1 Product Return Analysis

**Top 2 Returned Products:**

1. PAPER CRAFT, LITTLE BIRDIE — 80,995 units
2. MEDIUM CERAMIC TOP STORAGE JAR — 74,494 units

_Both are also the top 2 best-sellers, indicating a ~100% return rate._

#### 5.3.2 Return Rate by Product

- PAPER CRAFT, LITTLE BIRDIE — ~100%
- ROTATING SILVER ANGELS T-LIGHT HLDR — ~95%
- MEDIUM CERAMIC TOP STORAGE JAR — ~90%

_Multiple T-Light Holders appear → category-level quality issue._

---

### 5.4 Customer Analysis

#### 5.4.1 RFM Analysis

- **RFM** is a customer segmentation technique based on:
  - **Recency (R):** Days since last purchase → Fewer = Better
  - **Frequency (F):** Number of orders → More = Better
  - **Monetary (M):** Total spending → Higher = Better

#### 5.4.2 Calculate RFM

- **Total Customers:** 4,335
- **Recency (mean):** 93 days
- **Frequency (mean):** 4.25 orders
- **Monetary (mean):** £2,017

#### 5.4.3 RFM Scoring

- Scores from 1 to 5 for each metric.
- Top RFM Score: 555 (348 customers)
- Lowest RFM Score: 111 (180 customers)

#### 5.4.4 Customer Segmentation

- **Potential Loyalists:** 1,654
- **Champions:** 955
- **Loyal Customers:** 769
- **Lost Customers:** 638
- **New Customers:** 319

#### 5.4.5 Churn Risk Analysis

- **Customers at risk:** 862 (19.88%)
- **Revenue at risk:** £553,626
- **Threshold:** 180 days without purchase
- **Breakdown:**
  - Potential Loyalists: 667
  - Lost Customers: 195

---

### 5.5 Geographic Analysis

#### 5.5.1 Sales by Country

| Country        | Revenue    |
| -------------- | ---------- |
| United Kingdom | £8,726,769 |
| Netherlands    | £283,889   |
| EIRE           | £276,090   |
| Germany        | £205,381   |
| France         | £184,679   |

_UK dominates with ~87% of total revenue._

#### 5.5.2 Customers by Country

- **UK:** 3,917 customers (~90%)
- **Germany:** 94
- **France:** 87
- **Spain:** 30
- **Belgium:** 25

#### 5.5.3 Average Order Value by Country

| Country     | AOV     |
| ----------- | ------- |
| Netherlands | £122.26 |
| Australia   | £117.04 |
| Japan       | £116.56 |
| Sweden      | £86.25  |
| Denmark     | £49.62  |
| Lithuania   | £47.46  |
| Singapore   | £42.42  |
| Bahrain     | £41.90  |
| Lebanon     | £37.64  |
| Brazil      | £35.74  |

## _UK is absent from this list — despite being the largest market._

### 5.6 Cross-Analysis

#### 5.6.1 Country × Product

- **UK:** Gift and storage items
- **Netherlands & France:** RABBIT NIGHT LIGHT is #1 in both
- **EIRE:** Cake cases and Christmas items
- **Germany:** Snack boxes and silk fans

#### 5.6.2 Time × Customer

- **All segments:** Peak on **Thursday**
- **New Customers:** Peak on **Monday**
- **Friday:** Weakest for all
- **Average Hour:** 12–13 PM (uniform across segments)

#### 5.6.3 Customer × Returns

- **Loyal Customers:** Highest returns (£126)
- **Champions:** Low returns (£78)
- **New Customers:** Minimal returns (£2.66)

_Loyal Customers may need a return policy review._

\---

## 💡 6. Business Recommendations

Based on the analysis, here are actionable recommendations:

### 6.1 Product Strategy

1. **Urgent Quality Review for Top Sellers**
   - PAPER CRAFT, LITTLE BIRDIE and MEDIUM CERAMIC TOP STORAGE JAR show ~100% return rates.
   - **Action:** Investigate quality, packaging, and product descriptions immediately.

2. **T-Light Holder Category Review**
   - Multiple T-Light Holder products appear in high-return list.
   - **Action:** Audit this category for quality issues.

3. **Focus on High-Revenue Products**
   - REGENCY CAKESTAND 3 TIER, PAPER CRAFT, and WHITE HANGING HEART are top revenue generators.
   - **Action:** Ensure consistent stock availability and targeted marketing.

---

### 6.2 Customer Strategy

1. **Retain Champions (955 customers)**
   - They are the most valuable customers.
   - **Action:** VIP program, exclusive offers, and personalized communication.

2. **Convert Potential Loyalists (1,654 customers)**
   - Largest segment with high growth potential.
   - **Action:** Targeted campaigns to encourage repeat purchases.

3. **Win Back Lost Customers (638 customers)**
   - 862 customers are at churn risk (£553,626 revenue at risk).
   - **Action:** "We miss you" campaigns with special discounts.

4. **Reduce Loyal Customer Returns**
   - Loyal Customers have the highest return values (£126 avg).
   - **Action:** Review their purchase patterns and return policies.

---

### 6.3 Time Strategy

1. **Schedule Campaigns on Thursdays**
   - Thursday is the peak day for all segments.

2. **Target New Customers on Mondays**
   - New Customers peak on Mondays.

3. **Use Fridays for Operations**
   - Friday is the weakest day — ideal for restocking and maintenance.

4. **Peak Hours: 10 AM – 3 PM**
   - Schedule staff and customer support accordingly.

---

### 6.4 Geographic Strategy

1. **Maintain the UK Market**
   - UK generates ~87% of revenue.
   - **Action:** Continue investment while reducing dependency risk.

2. **Expand in High-Value Markets**
   - Netherlands, Australia, and Japan have the highest AOV.
   - **Action:** Increase marketing spend in these markets.

3. **Develop European Markets**
   - Netherlands, Germany, and France show strong potential.
   - **Action:** Localized inventory and marketing.

---

### 6.5 Inventory & Operations

1. **Localize Product Assortment**
   - Different countries prefer different products.
   - **Action:** Tailor inventory per market.

2. **Stock Top Sellers**
   - Ensure availability of top 5 products by quantity and revenue.

3. **Closed on Saturdays**
   - Store operates 6 days a week.
   - **Action:** Use Saturdays for logistics and restocking.

\---

## 📝 7. Executive Summary

### 7.1 Project Overview

This project analyzed 12 months of transactional data (December 2010 – December 2011) from a UK-based online retail store. The dataset contained **541,909 transactions** and **8 original columns**.

After cleaning and feature engineering, the final dataset contained **524,878 rows × 14 columns**, with **4,335 unique customers**.

---

### 7.2 Key Findings

#### Sales

- **Top Product:** REGENCY CAKESTAND 3 TIER (£174,156.54 revenue)
- **Top Product (Quantity):** PAPER CRAFT, LITTLE BIRDIE (80,995 units)
- **Top Customer:** Customer 14646 (£280,206.02 spent)

#### Time

- **Peak Month:** November (£1,453,295)
- **Peak Day:** Thursday (£2,133,746)
- **Peak Hour:** 12 PM (£1,414,006)
- **Weekday vs Weekend:** 92% vs 8%
- **Closed:** Saturdays

#### Customers

- **Total Customers:** 4,335
- **Average Recency:** 93 days
- **Average Frequency:** 4.25 orders
- **Average Monetary:** £2,017
- **Churn Risk:** 862 customers (19.88%) → £553,626 at risk

#### Products

- **Critical Issue:** Top 2 best-sellers have ~100% return rate
- **Category Issue:** Multiple T-Light Holders in high-return list

#### Geographic

- **UK Dominance:** ~87% of total revenue
- **Second Market:** Netherlands (£283,889)
- **High-Value Markets:** Netherlands, Australia, Japan (highest AOV)

#### Cross-Analysis

- **Country × Product:** Each market has unique preferences
- **Time × Customer:** All segments peak on Thursday
- **Customer × Returns:** Loyal Customers have highest returns

---

### 7.3 Critical Insights

1. **Product Quality Issue**
   - PAPER CRAFT and MEDIUM CERAMIC JAR have ~100% return rates.
   - These are also the top 2 best-sellers, indicating a serious quality or description issue.

2. **UK Concentration Risk**
   - 87% of revenue from a single market.
   - High dependency poses strategic risk.

3. **Churn Risk**
   - ~20% of customers are at risk of leaving.
   - £553,626 in revenue is at stake.

4. **Loyal Customer Returns**
   - Loyal Customers have the highest return values (£126 avg).
   - Their return patterns differ from Champions.

---

### 7.4 Recommended Actions

| Priority | Action                                  | Expected Impact                 |
| -------- | --------------------------------------- | ------------------------------- |
| **1**    | Quality review for top 2 products       | Reduce returns, protect revenue |
| **2**    | Retain Champions with VIP program       | Protect key revenue source      |
| **3**    | Convert Potential Loyalists             | Increase revenue                |
| **4**    | Win back Lost Customers                 | Recover £553K at risk           |
| **5**    | Expand in Netherlands, Australia, Japan | Reduce UK dependency            |
| **6**    | Schedule campaigns on Thursdays         | Maximize reach                  |

---

### 7.5 Conclusion

The online retail store has strong fundamentals:

- Loyal customer base (Champions, Loyal Customers)
- Consistent sales patterns
- High-value products

**However**, critical issues require immediate attention:

- Product quality (returns)
- Market concentration (UK)
- Customer churn

By addressing these issues and implementing the recommendations, the store can:

- **Reduce revenue loss** from returns and churn
- **Increase revenue** by expanding high-value markets
- **Sustain growth** through a diversified customer and geographic base.

\---

## 🔗 8. References

## 8. References

- **Dataset:** [UCI Online Retail Dataset](https://archive.ics.uci.edu/ml/datasets/online+retail)
- **Repository:** [Your GitHub Link]
- **Tools:** Python, Pandas, NumPy, Matplotlib, Seaborn
- **Author:** Aryam Khaled
- **Date:** September 2026
