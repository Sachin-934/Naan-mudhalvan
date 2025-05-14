from google.colab import files
uploaded = files.upload()
print(uploaded)
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
import plotly.express as px
import warnings
warnings.filterwarnings(action = "ignore")
data = pd.read_csv("/content/Amazon Sale Report.csv")
data.shape
data['Date'] = pd.to_datetime(data['Date'], errors='coerce')
data.isnull().sum()
data['Amount'].fillna(data['Amount'].mean(), inplace = True)
data['ship-postal-code'].fillna(data['ship-postal-code'].mean(), inplace = True)
data['currency'].fillna(data['currency'].mode()[0], inplace = True)
data['fulfilled-by'].fillna(data['fulfilled-by'].mode()[0], inplace = True)
data['ship-state'].fillna(data['ship-state'].mode()[0], inplace = True)
data['ship-country'].fillna(data['ship-country'].mode()[0], inplace = True)
data['ship-city'].fillna(data['ship-city'].mode()[0], inplace = True)
data.drop_duplicates(inplace = True)
data.columns
**EXPLORATORY DATA ANALYSIS**
sns.pairplot(data, hue = 'currency')
plt.show()
for i in data.select_dtypes(include = "number").columns:
    fig = px.histogram(data, x = i)
    fig.show()
for i in data.select_dtypes(include = "number").columns:
    fig = px.box(data, x = i)
    fig.show()
**    line chart**
sales_per_day = data.groupby('Date')['Amount'].sum().reset_index()
fig = px.line(sales_per_day, x = 'Date', y = 'Amount',
              title = 'Sales Amount Over Time')
fig.show()
fig = px.pie(data, names = 'Sales Channel',
             title = 'Sales Channel Distribution', hole=0.4)
fig.show()
fig = px.histogram(data, x = 'Qty',
                   title = 'Quantity Distribution')
fig.show()
category_sales = data.groupby('Category')['Amount'].sum().reset_index()

fig = px.bar(category_sales, x = 'Category', y = 'Amount',
             title = 'Category-wise Sales', text='Amount')
fig.show()
fig = px.box(data, x = 'Fulfilment', y = 'Amount',
             title = 'Sales Amount by Fulfillment Type')
fig.show()
fig = px.scatter(data, x = 'Qty', y = 'Amount',
                 title = 'Quantity vs. Amount')
fig.show()
sns.set(rc = {"figure.figsize" : (22, 12)})
s = data.select_dtypes(include = "number").corr()
sns.heatmap(s, annot = True, cmap = "coolwarm", linewidth = 0.5)
plt.title("Correlation Heatmap", fontsize = 25, fontweight = "bold")
plt.xticks(fontsize = 15)
plt.yticks(fontsize = 15)
plt.show()
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
df = pd.read_csv("/content/Amazon_Sale_Report_With_Satisfaction.csv")
print("Summary Stats:")
print(df["Customer Satisfaction Score"].describe())

# 2. Frequency of each score
print("\nScore Frequency:")
print(df["Customer Satisfaction Score"].value_counts().sort_index())

# 3. Histogram
plt.figure(figsize=(8, 5))
sns.histplot(df["Customer Satisfaction Score"], bins=10, kde=True, color='skyblue')
plt.title("Distribution of Customer Satisfaction Score")
plt.xlabel("Score")
plt.ylabel("Frequency")
plt.grid(True)
plt.show()

# 4. Boxplot for outlier check
plt.figure(figsize=(6, 4))
sns.boxplot(x=df["Customer Satisfaction Score"], color='orange')
plt.title("Boxplot of Customer Satisfaction Score")
plt.show()
import pandas as pd
import matplotlib.pyplot as plt

# Load the CSV file
df = pd.read_csv("/content/Amazon_Sale_Report_With_Satisfaction.csv")

# Clean column names by stripping whitespace
df.columns = df.columns.str.strip()

# Use the correct column name
score_counts = df['Customer Satisfaction Score'].value_counts().sort_index()

# Plotting pie chart
plt.figure(figsize=(8, 8))
plt.pie(score_counts, labels=score_counts.index, autopct='%1.1f%%', startangle=90)
plt.title('Customer Satisfaction Score Distribution')
plt.axis('equal')
plt.show()
