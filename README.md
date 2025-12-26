import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load the dataset
data = pd.read_csv("data.csv", encoding='latin1')  # Use 'latin1' to avoid encoding errors

# Show the first 5 rows
data.head()

# Check columns and data info
print(data.columns)
data.info()

# Check for missing values
print(data.isnull().sum())

# clening the data
# Remove duplicate rows
data.drop_duplicates(inplace=True)

# Convert InvoiceDate to datetime format
data['InvoiceDate'] = pd.to_datetime(data['InvoiceDate'])

# Drop rows where CustomerID is missing
data = data.dropna(subset=['CustomerID'])

# Check again after cleaning
print(data.info())

#Find Top 10 Best-Selling Products

# Group by product and sum the quantity sold
top_products = data.groupby('Description')['Quantity'].sum().sort_values(ascending=False).head(10)

# Show top products
print(top_products)

#Visualize Top 10 Products Sold
import matplotlib.pyplot as plt

plt.figure(figsize=(10,6))
plt.barh(top_products.index, top_products.values, color='mediumseagreen')
plt.title('Top 10 Best-Selling Products')
plt.xlabel('Quantity Sold')
plt.ylabel('Product Description')
plt.gca().invert_yaxis()  # So the top product is on top
plt.show()

#Monthly Revenue Trend
# Calculate revenue column
data['Revenue'] = data['Quantity'] * data['UnitPrice']

# Group revenue by month
monthly_revenue = data.groupby(data['InvoiceDate'].dt.to_period('M'))['Revenue'].sum()

# Convert to datetime for plotting
monthly_revenue.index = monthly_revenue.index.to_timestamp()

# Plot monthly revenue
plt.figure(figsize=(10,6))
monthly_revenue.plot(marker='o')
plt.title('Monthly Revenue Trend')
plt.xlabel('Month')
plt.ylabel('Revenue')
plt.grid(True)
plt.show()

#Top 10 Customers by Revenue

# Group by CustomerID and sum revenue
top_customers = data.groupby('CustomerID')['Revenue'].sum().sort_values(ascending=False).head(10)

# Show top customers
print(top_customers)

#Visualize Top Customers

plt.figure(figsize=(10,6))
plt.barh(top_customers.index.astype(str), top_customers.values, color='coral')
plt.title('Top 10 Customers by Revenue')
plt.xlabel('Revenue')
plt.ylabel('Customer ID')
plt.gca().invert_yaxis()  # So the top customer is on top
plt.show()

#Country-wise Revenue
# Group revenue by country
country_revenue = data.groupby('Country')['Revenue'].sum().sort_values(ascending=False).head(10)

# Plot top 10 countries
plt.figure(figsize=(10,6))
plt.barh(country_revenue.index, country_revenue.values, color='skyblue')
plt.title('Top 10 Countries by Revenue')
plt.xlabel('Revenue')
plt.ylabel('Country')
plt.gca().invert_yaxis()
plt.show()

