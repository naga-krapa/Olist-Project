# Olist Data Analyst Project

### Tools you'll use

```text
Python
├── Pandas
├── NumPy
├── Matplotlib
└── Seaborn

Jupyter Notebook / VS Code
Git + GitHub
SQL (optional but highly recommended)
Power BI (final dashboard)
```

Your project will eventually look like:

```text
Olist E-Commerce Analytics
        │
        ├── Data Understanding
        ├── Data Cleaning
        ├── Data Transformation
        ├── Data Integration
        ├── EDA
        ├── Business Analysis
        ├── Visualization
        ├── SQL Analysis
        ├── Power BI Dashboard
        └── Business Recommendations
```

---

# WEEK 1 — Understand + Clean the Data

## DAY 1 — Understand the Olist dataset

### Goal

Don't start coding immediately. First understand what each table represents.

Typical Olist tables include:

```text
olist_orders_dataset
olist_order_items_dataset
olist_order_payments_dataset
olist_order_reviews_dataset
olist_products_dataset
olist_customers_dataset
olist_sellers_dataset
olist_geolocation_dataset
product_category_name_translation
```

Create a project folder:

```text
olist-project/
│
├── data/
├── notebooks/
├── sql/
├── visuals/
├── reports/
└── README.md
```

Load the datasets:

```python
import pandas as pd

orders = pd.read_csv("data/olist_orders_dataset.csv")
items = pd.read_csv("data/olist_order_items_dataset.csv")
payments = pd.read_csv("data/olist_order_payments_dataset.csv")
reviews = pd.read_csv("data/olist_order_reviews_dataset.csv")
products = pd.read_csv("data/olist_products_dataset.csv")
customers = pd.read_csv("data/olist_customers_dataset.csv")
sellers = pd.read_csv("data/olist_sellers_dataset.csv")
```

### Questions — Day 1

Answer:

1. How many tables are there?
2. What does each table represent?
3. How many rows are in each table?
4. How many columns are in each table?
5. What are the primary keys?
6. Which columns connect the tables?

**GitHub task:** Create your first commit.

```bash
git add .
git commit -m "Initial Olist project setup"
git push
```

---

# DAY 2 — Data Inspection

Learn to inspect every table.

Use:

```python
df.head()
df.tail()
df.shape
df.columns
df.info()
df.dtypes
df.describe()
```

For each dataset, determine:

* numerical columns
* categorical columns
* date columns
* ID columns

### Questions — Day 2

7. How many orders are in the dataset?

8. How many customers?

9. How many sellers?

10. How many products?

11. How many order items?

12. What is the date range of the orders?

13. Which columns contain dates?

14. Which columns contain missing values?

15. Which columns contain IDs?

### GitHub

```bash
git add .
git commit -m "Added initial data exploration"
git push
```

---

# DAY 3 — Missing Values

This is very important for interviews.

Learn:

```python
df.isna().sum()
df.isna().mean() * 100
```

Create a missing-value summary.

Example:

```python
missing = orders.isna().sum().sort_values(ascending=False)
missing
```

### Questions — Day 3

16. Which columns have missing values?

17. What percentage of values are missing?

18. Which missing values should be removed?

19. Which missing values should be retained?

20. Can missing values themselves provide business information?

For example, some order status/date fields may naturally be missing depending on what happened to an order.

**Important:** Don't blindly fill every null with 0.

---

# DAY 4 — Duplicates + Data Types

Learn:

```python
df.duplicated().sum()
df.drop_duplicates()
```

Date conversion:

```python
orders["order_purchase_timestamp"] = pd.to_datetime(
    orders["order_purchase_timestamp"]
)
```

Check:

```python
orders.dtypes
```

### Questions — Day 4

21. Are there duplicate rows?

22. Are there duplicate IDs?

23. Which columns need datetime conversion?

24. Are numerical columns stored correctly?

25. Are categorical columns stored correctly?

26. Are there suspicious values?

---

# DAY 5 — Understand Relationships Between Tables

This is one of the **most important days**.

Understand:

```text
orders
   │
   ├── order_id
   ↓
order_items
   │
   ├── product_id
   ↓
products

order_items
   │
   ├── seller_id
   ↓
sellers

orders
   │
   ├── customer_id
   ↓
customers

orders
   │
   ├── order_id
   ↓
payments

orders
   │
   ├── order_id
   ↓
reviews
```

Practice:

```python
df = orders.merge(
    customers,
    on="customer_id",
    how="left"
)
```

Then:

```python
df = orders.merge(
    items,
    on="order_id",
    how="inner"
)
```

### Questions — Day 5

27. What is the relationship between orders and customers?

28. What is the relationship between orders and payments?

29. What is the relationship between orders and order items?

30. What is the relationship between products and order items?

31. What is the relationship between sellers and order items?

32. Which table should be the starting point for revenue analysis?

This day will also help you understand the `merge()` errors you've encountered before.

---

# DAY 6 — Build Your Master Dataset

Now start creating your analytical dataset.

For example:

```python
master = orders.merge(
    items,
    on="order_id",
    how="left"
)

master = master.merge(
    products,
    on="product_id",
    how="left"
)

master = master.merge(
    customers,
    on="customer_id",
    how="left"
)
```

Then bring in payments/reviews where appropriate.

**Don't blindly merge everything together.**

Because some tables have multiple rows per order, careless joins can duplicate revenue.

This is an important real-world Data Analyst skill.

### Day 6 questions

33. What happens to row count after each merge?

34. Did any rows become duplicated?

35. Is `order_id` unique after each merge?

36. Is revenue being duplicated?

37. Which tables should be aggregated before merging?

---

# DAY 7 — Review + GitHub

Don't learn new concepts today.

Review:

```text
Python
Pandas
merge
groupby
missing values
duplicates
datetime
data types
```

Clean your notebook.

Your GitHub should now contain:

```text
README.md
notebooks/
data/
```

Don't upload huge raw datasets if their licensing/distribution terms don't permit it; you can document where the dataset comes from instead.

---

# WEEK 2 — EDA + Business Questions

Now the project becomes much more interesting.

# DAY 8 — Sales Overview

Calculate:

### Total orders

```python
orders["order_id"].nunique()
```

### Total customers

```python
customers["customer_unique_id"].nunique()
```

### Revenue

Use the appropriate item-level amount after checking the data model.

### Average order value

```text
Total Revenue / Number of Orders
```

### Questions

38. What is total revenue?

39. How many orders were placed?

40. How many unique customers are there?

41. What is the average order value?

42. What is the average number of items per order?

43. How many sellers are there?

---

# DAY 9 — Time Analysis

Create:

```python
orders["year"] = orders["order_purchase_timestamp"].dt.year
orders["month"] = orders["order_purchase_timestamp"].dt.month
orders["year_month"] = orders["order_purchase_timestamp"].dt.to_period("M")
```

Analyze:

```python
orders.groupby("year")["order_id"].nunique()
```

### Questions

44. How do orders change by year?

45. How do orders change by month?

46. Which month has the highest number of orders?

47. Which month has the highest revenue?

48. Is there seasonal behavior?

### Visualization

Line chart:

```python
sns.lineplot(...)
```

---

# DAY 10 — Product Analysis

Analyze product categories.

Questions:

49. Which categories have the most orders?

50. Which categories generate the most revenue?

51. Which categories have the highest average order value?

52. Which categories have the highest average freight cost?

53. Which categories have the best review scores?

54. Which categories have the lowest review scores?

Use:

```python
groupby()
agg()
sort_values()
```

---

# DAY 11 — Customer Analysis

Analyze:

```text
customer_state
customer_city
customer_unique_id
```

Questions:

55. Which states have the most customers?

56. Which states generate the most revenue?

57. Which cities have the most customers?

58. What is the average order value by state?

59. Which states have higher/lower review scores?

Visualization:

```text
Top 10 states by customers
Top 10 states by revenue
```

---

# DAY 12 — Seller Analysis

Analyze sellers.

Questions:

60. Which sellers have the most orders?

61. Which sellers generate the most revenue?

62. What is the average order value by seller?

63. Which sellers have higher review scores?

64. Which sellers have higher freight costs?

Don't just produce a "Top 10" list—explain what the result means.

---

# DAY 13 — Payment Analysis

Use:

```text
payment_type
payment_installments
payment_value
```

Questions:

65. Which payment method is most common?

66. Which payment method generates the most revenue?

67. What is the average payment value by payment method?

68. How are installment counts distributed?

69. Is there a relationship between installments and order value?

Useful visualization:

```text
Bar chart
Pie/donut chart
Boxplot
```

---

# DAY 14 — Review + Delivery Analysis

This is a **very valuable part of the project**.

Calculate:

```text
purchase → approval
purchase → shipping
purchase → delivery
estimated delivery → actual delivery
```

Questions:

70. What is the average delivery time?

71. What percentage of orders are delivered late?

72. Which categories have longer delivery times?

73. Which states have longer delivery times?

74. Do late deliveries have lower review scores?

75. What is the relationship between delivery time and review score?

This gives you a strong business story.

---

# WEEK 3 — Advanced EDA + Dashboard + Portfolio

# DAY 15 — Correlation + Multivariate Analysis

Analyze relationships between numerical variables.

For example:

```python
corr = df.corr(numeric_only=True)
```

Heatmap:

```python
sns.heatmap(corr, annot=True)
plt.show()
```

Questions:

76. Which numerical variables are correlated?

77. Is freight value related to product price?

78. Is delivery time related to review score?

79. Is payment value related to installments?

Remember:

> Correlation does not prove causation.

---

# DAY 16 — Advanced EDA

Now use:

```python
pivot_table()
crosstab()
groupby()
```

Examples:

```python
pd.crosstab(
    df["customer_state"],
    df["payment_type"]
)
```

And:

```python
df.pivot_table(
    index="customer_state",
    columns="payment_type",
    values="payment_value",
    aggfunc="sum"
)
```

### Questions

80. Which payment methods are preferred by different states?

81. Which categories perform differently across regions?

82. Which combinations of category/payment/state produce interesting patterns?

This is where your earlier Pandas practice becomes useful.

---

# DAY 17 — Create 8–10 Strong Visualizations

Don't make 50 random charts.

Create a focused dashboard/story.

For example:

### 1. Monthly revenue

### 2. Monthly orders

### 3. Top 10 product categories

### 4. Revenue by state

### 5. Payment method distribution

### 6. Review score distribution

### 7. Delivery time distribution

### 8. Delivery time vs review score

### 9. Top sellers

### 10. Revenue/category comparison

Use:

```python
Matplotlib
Seaborn
```

---

# DAY 18 — SQL Analysis

Since you're targeting Data Analyst positions, I strongly recommend adding SQL.

Take the cleaned/loaded data into a SQL database and reproduce important analyses.

Practice:

```sql
SELECT
GROUP BY
ORDER BY
WHERE
HAVING
CASE
JOIN
CTE
WINDOW FUNCTIONS
```

Example:

```sql
SELECT
    customer_state,
    COUNT(DISTINCT customer_unique_id) AS customers
FROM customers
GROUP BY customer_state
ORDER BY customers DESC;
```

Then practice joins:

```sql
SELECT
    p.product_category_name,
    SUM(oi.price) AS revenue
FROM order_items oi
JOIN products p
    ON oi.product_id = p.product_id
GROUP BY p.product_category_name
ORDER BY revenue DESC;
```

This will make your project much stronger for interviews.

---

# DAY 19 — Power BI Dashboard

Create a professional dashboard.

### Page 1 — Executive Overview

Include:

```text
Total Revenue
Total Orders
Total Customers
Average Order Value
Average Review Score
Late Delivery %
```

### Page 2 — Sales

```text
Monthly Revenue
Monthly Orders
Top Categories
Top Sellers
```

### Page 3 — Customers

```text
Customers by State
Revenue by State
Average Order Value
```

### Page 4 — Delivery & Reviews

```text
Delivery Time
Late Delivery %
Review Scores
Delivery vs Reviews
```

---

# DAY 20 — Business Insights

This is extremely important.

Don't finish with:

> "Category X has the highest sales."

Turn it into a business insight.

For example:

```text
Finding:
Category X generated the highest revenue.

Possible business implication:
The company could investigate whether inventory,
marketing, and seller capacity should be prioritized
for high-performing categories.
```

Separate **what the data shows** from **what you recommend**.

Create approximately **8–12 major insights**.

---

# DAY 21 — Finalize Your Portfolio

Your GitHub repository should look something like:

```text
olist-ecommerce-analysis/
│
├── README.md
│
├── data/
│
├── notebooks/
│   ├── 01_data_understanding.ipynb
│   ├── 02_data_cleaning.ipynb
│   ├── 03_data_integration.ipynb
│   ├── 04_eda.ipynb
│   └── 05_business_analysis.ipynb
│
├── sql/
│   └── olist_analysis.sql
│
├── visuals/
│
└── powerbi/
    └── olist_dashboard.pbix
```

Your README should explain:

```text
1. Project Overview
2. Business Problem
3. Dataset
4. Tools Used
5. Data Cleaning
6. Data Modeling
7. EDA
8. Key Questions
9. Key Insights
10. Dashboard
11. Business Recommendations
```

---

# How many questions should you actually solve?

Don't worry about solving all **82 questions** above.

I gave you a large question bank so you can practice.

For your **final portfolio**, I recommend selecting approximately:

### **35–40 important questions**

A good distribution would be:

| Area               | Questions |
| ------------------ | --------: |
| Data understanding |         5 |
| Data quality       |         5 |
| Sales              |         5 |
| Products           |         5 |
| Customers          |         4 |
| Sellers            |         3 |
| Payments           |         3 |
| Delivery           |         5 |
| Reviews            |         3 |
| Advanced analysis  |         4 |
| **Total**          |   **~42** |

You don't have to force exactly 42. **Quality matters more than the number.**

---

# Your daily GitHub routine

Since you told me you want to practice GitHub every day, use this workflow:

```text
Start work
   ↓
git pull
   ↓
Work on notebook/code
   ↓
Test everything
   ↓
git status
   ↓
git add .
   ↓
git commit -m "meaningful message"
   ↓
git push
```

For example:

```bash
git pull

# work...

git status
git add .
git commit -m "Added missing value analysis"
git push
```

This gives you **daily Git practice while building the actual project**.

---

# Most important: don't just copy code

For every question, follow this pattern:

```text
Business Question
       ↓
What data do I need?
       ↓
Which tables?
       ↓
Do I need merge?
       ↓
Do I need cleaning?
       ↓
Which Pandas function?
       ↓
Calculate
       ↓
Visualize
       ↓
Interpret
       ↓
Business insight
```

For example:

**Question:** Which product categories generate the most revenue?

Think:

```text
Need revenue
     ↓
order_items → price
     ↓
product_id
     ↓
products → category
     ↓
merge
     ↓
groupby(category)
     ↓
sum(price)
     ↓
sort_values()
     ↓
bar chart
     ↓
business insight
```

That thinking process is **much more valuable for your interviews** than memorizing Pandas syntax.

### Your target after 21 days

You should be able to say in an interview:

> "I worked on an e-commerce analytics project using the Olist dataset. I cleaned and integrated multiple relational datasets, performed exploratory analysis using Python/Pandas, analyzed sales, customers, products, payments, delivery performance and reviews, created visualizations, performed SQL analysis, and built a Power BI dashboard."

That is a **much stronger portfolio story** than simply saying "I did a Pandas project."

And because you've already been practicing `merge`, `groupby`, `crosstab`, `pivot`, `datetime`, `loc/iloc`, missing values, correlation, Matplotlib and Seaborn, **a lot of the skills you need for this project are exactly the ones you've been learning.**















For **your Olist project, I recommend using BOTH — but with different purposes.**

### 🟢 Use Jupyter Notebook for the actual analysis

This is where you should do most of your Olist work:

```text
Jupyter Notebook
    ↓
Load data
    ↓
Explore data
    ↓
Clean data
    ↓
Merge tables
    ↓
EDA
    ↓
Answer business questions
    ↓
Create charts
    ↓
Write insights
```

For example:

```python
import pandas as pd

orders = pd.read_csv("../data/olist_orders_dataset.csv")

orders.head()
orders.info()
orders.isna().sum()
```

You can see the output immediately, which is very useful while you're learning.

---

### 🔵 Use VS Code for the overall project

Use VS Code to manage the **whole project**:

```text
olist-project/
│
├── data/
├── notebooks/
├── sql/
├── visuals/
├── README.md
└── requirements.txt
```

Your notebooks can be inside:

```text
notebooks/
    01_data_understanding.ipynb
    02_data_cleaning.ipynb
    03_data_integration.ipynb
    04_eda.ipynb
    05_business_analysis.ipynb
```

VS Code can also open and run `.ipynb` notebooks, so you don't necessarily need the separate Jupyter application.

---

## ⭐ What I recommend specifically for you

Since you're **learning while building the project**, start with:

**VS Code + Jupyter Notebook (`.ipynb`)**

That gives you both:

| Task                  | Use              |
| --------------------- | ---------------- |
| Write/run analysis    | Jupyter Notebook |
| See DataFrames/output | Jupyter Notebook |
| EDA                   | Jupyter Notebook |
| Charts                | Jupyter Notebook |
| Organize project      | VS Code          |
| Git/GitHub            | VS Code          |
| SQL files             | VS Code          |
| README                | VS Code          |
| Final portfolio       | GitHub           |

### Your workflow

```text
VS Code
   │
   ├── notebooks/
   │      └── Olist_analysis.ipynb  ← MOST OF YOUR WORK
   │
   ├── sql/
   │      └── analysis.sql
   │
   ├── visuals/
   │
   ├── README.md
   │
   └── data/
   │
   ↓
Git
   ↓
GitHub
```

**Don't create a separate `.py` file for every small analysis right now.** Since you're still learning Pandas/EDA, notebooks will make it much easier to experiment, see outputs, fix errors, and document your thinking.

Once the project is complete, we can organize it professionally for GitHub and your resume.

