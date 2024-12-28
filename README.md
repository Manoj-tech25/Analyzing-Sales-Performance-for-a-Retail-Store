# Online Retail Data Analysis
This project analyzes an Online Retail dataset to gain insights into customer purchasing patterns and product popularity. The dataset contains information about transactions, products, customers, and sales.

# Objective
The analysis aims to:

Clean the dataset to ensure accuracy and usability.
Explore customer behavior (spending and frequency).
Identify popular products based on quantity sold and revenue generated.
Provide actionable business insights.
# Data Cleaning Steps
Removed rows with missing CustomerID values to ensure customer-level analysis accuracy.
Filled missing product descriptions with the placeholder "Unknown".
Converted InvoiceDate to a proper datetime format for time-based analysis.
Added a new column TotalPrice = Quantity × UnitPrice.
# Exploratory Data Analysis (EDA)
1. Top 10 Countries by Total Sales
The United Kingdom contributes the majority of sales, followed by a few other countries.
Visualized using a horizontal bar chart.
2. Monthly Sales Trends
Sales fluctuate across months, indicating seasonal demand or event-specific peaks.
Plotted a line chart to show trends.
3. Customer Purchasing Patterns
Identified Top 10 High-Value Customers (those with the highest total spending).
Analyzed customer purchase frequency and spending to segment customers.
4. Product Popularity
Highlighted the Top 10 Most Purchased Products (by quantity).
Identified the Top 10 Revenue-Generating Products to focus on high-value items.
# Key Insights
## Customer Behavior:
High-value customers significantly contribute to total revenue.
Retaining these customers is critical for sustained growth.
## Product Popularity:
Most purchased products (high volume) may differ from high-revenue products (more expensive).
Optimize stock levels and promotions based on product performance.
## Country-Wise Sales:
The United Kingdom is the dominant market, suggesting a strong local customer base.
# Code
Below is the Python code used for data cleaning and analysis:

# python

# Import libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load dataset
data = pd.read_csv('OnlineRetail.csv', encoding='latin1')

# Data Cleaning
data_cleaned = data.dropna(subset=['CustomerID']).copy()
data_cleaned['Description'].fillna('Unknown', inplace=True)
data_cleaned['InvoiceDate'] = pd.to_datetime(data_cleaned['InvoiceDate'], errors='coerce')
data_cleaned['TotalPrice'] = data_cleaned['Quantity'] * data_cleaned['UnitPrice']

# Plotting Top 10 Countries by Sales
top_countries = data_cleaned.groupby('Country')['TotalPrice'].sum().sort_values(ascending=False).head(10)
plt.figure(figsize=(10, 6))
sns.barplot(x=top_countries.values, y=top_countries.index, palette="viridis")
plt.title("Top 10 Countries by Total Sales", fontsize=16)
plt.xlabel("Total Sales (in currency)", fontsize=12)
plt.ylabel("Country", fontsize=12)
plt.show()

# Plotting Monthly Sales Trends
data_cleaned['Month'] = data_cleaned['InvoiceDate'].dt.to_period('M')
monthly_sales = data_cleaned.groupby('Month')['TotalPrice'].sum()
plt.figure(figsize=(14, 6))
monthly_sales.plot(kind='line', marker='o', color='dodgerblue', linewidth=2)
plt.title("Monthly Sales Trends", fontsize=16)
plt.xlabel("Month", fontsize=12)
plt.ylabel("Total Sales (in currency)", fontsize=12)
plt.grid(True)
plt.show()

# Customer Purchasing Patterns
customer_data = data_cleaned.groupby('CustomerID').agg(
    TotalSpent=('TotalPrice', 'sum'),
    PurchaseFrequency=('InvoiceNo', 'nunique')
).sort_values(by='TotalSpent', ascending=False)

# Plotting Top 10 High-Value Customers
top_customers = customer_data.head(10)
plt.figure(figsize=(10, 6))
sns.barplot(x=top_customers.TotalSpent, y=top_customers.index, palette="coolwarm")
plt.title("Top 10 High-Value Customers by Total Spending", fontsize=16)
plt.xlabel("Total Spending (in currency)", fontsize=12)
plt.ylabel("Customer ID", fontsize=12)
plt.show()

# Product Popularity Analysis
product_popularity = data_cleaned.groupby('Description').agg(
    TotalQuantity=('Quantity', 'sum'),
    TotalRevenue=('TotalPrice', 'sum')
).sort_values(by='TotalQuantity', ascending=False).head(10)

# Plotting Most Purchased Products
plt.figure(figsize=(12, 6))
sns.barplot(x=product_popularity.TotalQuantity, y=product_popularity.index, palette="crest")
plt.title("Top 10 Most Purchased Products by Quantity", fontsize=16)
plt.xlabel("Total Quantity Purchased", fontsize=12)
plt.ylabel("Product Description", fontsize=12)
plt.show()

# Plotting Top Revenue-Generating Products
top_revenue_products = product_popularity.sort_values(by='TotalRevenue', ascending=False).head(10)
plt.figure(figsize=(12, 6))
sns.barplot(x=top_revenue_products.TotalRevenue, y=top_revenue_products.index, palette="mako")
plt.title("Top 10 Revenue-Generating Products", fontsize=16)
plt.xlabel("Total Revenue (in currency)", fontsize=12)
plt.ylabel("Product Description", fontsize=12)
plt.show()
Conclusion
The analysis provides actionable insights for businesses to:

Identify and retain high-value customers.
Optimize inventory management based on product performance.
Focus marketing efforts on top-performing regions and products.


