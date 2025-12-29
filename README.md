# Sales-Data-Analysis-Using-Python-Pandas-NumPy-Matplotlib-Seaborn-
This project performs end-to-end exploratory data analysis on a retail transactions dataset, focusing on sales trends, customer behavior, revenue analysis, and cancellations.
🛠️ Tools & Libraries Used

Python

Pandas

NumPy

Matplotlib

Seaborn

📁 Dataset

Retail transaction dataset containing:

InvoiceNo

StockCode

Description

Quantity

InvoiceDate

UnitPrice

CustomerID

Country

🔹 Import Required Libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

Q1. Basic Data Exploration
df = pd.read_csv("online_retail.csv", encoding="ISO-8859-1")

# Rows & Columns
df.shape

# Missing values
df.isnull().sum()

# Data types
df.dtypes

Q2. Time Period & Transactions
df["InvoiceDate"] = pd.to_datetime(df["InvoiceDate"])

earliest_date = df["InvoiceDate"].min()
latest_date = df["InvoiceDate"].max()

unique_invoices = df["InvoiceNo"].nunique()

Q3. Revenue Calculation
df["TotalPrice"] = df["Quantity"] * df["UnitPrice"]

total_revenue = df["TotalPrice"].sum()
avg_revenue_per_transaction = df.groupby("InvoiceNo")["TotalPrice"].sum().mean()

Q4. Monthly Sales Trend
df["Month"] = df["InvoiceDate"].dt.to_period("M")

monthly_sales = df.groupby("Month")["TotalPrice"].sum()

plt.figure()
monthly_sales.plot()
plt.title("Monthly Revenue Trend")
plt.xlabel("Month")
plt.ylabel("Total Revenue")
plt.show()

Q5. Top 10 Best-Selling Products
top_products = (
    df.groupby("Description")["TotalPrice"]
    .sum()
    .sort_values(ascending=False)
    .head(10)
)

plt.figure()
top_products.plot(kind="bar")
plt.title("Top 10 Best-Selling Products")
plt.ylabel("Revenue")
plt.show()

Q6. Country-Wise Sales (Excluding UK)
country_sales = (
    df[df["Country"] != "United Kingdom"]
    .groupby("Country")["TotalPrice"]
    .sum()
    .sort_values(ascending=False)
    .head(5)
)

plt.figure()
country_sales.plot(kind="barh")
plt.title("Top 5 Countries by Revenue (Excl. UK)")
plt.xlabel("Revenue")
plt.show()

Q7. Customer Behavior Analysis
unique_customers = df["CustomerID"].nunique()

top_customers = (
    df.groupby("CustomerID")["TotalPrice"]
    .sum()
    .sort_values(ascending=False)
    .head(5)
)

avg_spend_per_customer = (
    df.groupby("CustomerID")["TotalPrice"].sum().mean()
)

Q8. Month-on-Month Growth Rate (NumPy)
monthly_sales_df = monthly_sales.reset_index()
monthly_sales_df["GrowthRate(%)"] = (
    monthly_sales_df["TotalPrice"].pct_change() * 100
)

monthly_sales_df

Q9. Correlation Analysis
corr = df[["Quantity", "UnitPrice", "TotalPrice"]].corr()

sns.heatmap(corr, annot=True, cmap="coolwarm")
plt.title("Correlation Heatmap")
plt.show()

Q10. Returns / Cancellations Detection
cancelled = df[df["InvoiceNo"].astype(str).str.startswith("C")]

total_cancelled_invoices = cancelled["InvoiceNo"].nunique()
lost_revenue = cancelled["TotalPrice"].sum()

🎯 Key Insights

Monthly sales show seasonal patterns

A small number of customers contribute significantly to revenue

Cancellations result in noticeable revenue loss

Quantity strongly affects TotalPrice

Certain products dominate overall sales

👨‍💻 Author

Name: Faizan Bhat
Course: Data Analytics / Python Programming
Skills: Pandas, NumPy, Data Visualization, EDA
