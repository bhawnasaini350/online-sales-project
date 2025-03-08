**Online Sales- Only Pandas Project**
Skills : Pandas (Filtering , Groupby , Function In Pandas , Aggregate Functions)
So , we will be going through some of the analysis in this online shop customers sales. As you must have seen the value in columns ,
most of them are in numbers. So what we will do is to convert those numerical columns to Categorical columns for better understanding.

Here's a line-by-line explanation of my code:

---

### **1. Importing Necessary Libraries**
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```
- **`pandas`**: Used for data manipulation and analysis.
- **`numpy`**: Used for numerical operations.
- **`matplotlib.pyplot`**: Used for data visualization (charts/graphs).
- **`seaborn`**: Enhances visualization aesthetics.

---

### **2. Reading the Data**
```python
df = pd.read_csv('Online Shop Customer Sales Data.csv')
```
- Loads the dataset into a Pandas DataFrame.

---

### **3. Mapping Encoded Values to Meaningful Labels**
#### **Gender Mapping**
```python
def gender(a):
    if a == 0:
        return 'Male'
    else:
        return 'Female'

df['Gender'] = df['Gender'].apply(gender)
```
- Converts `0` to `"Male"` and `1` to `"Female"` in the `Gender` column.

#### **Payment Method Mapping**
```python
def payment(b):
    if b == 0:
        return 'Digital Wallets'
    elif b == 1:
        return 'Card'
    elif b == 2:
        return 'Paypal'
    else:
        return 'Other'

df['Pay_Method'] = df['Pay_Method'].apply(payment)
```
- Converts numerical codes into readable payment method names.

#### **Browser Mapping**
```python
def browser(c):
    if c == 0:
        return 'Chrome'
    elif c == 1:
        return 'Safari'
    elif c == 2:
        return 'Edge'
    else:
        return 'Other'

df['Browser'] = df['Browser'].apply(browser)
```
- Converts browser codes into their respective names.

#### **Newsletter Subscription Mapping**
```python
def Newsletter(d):
    if d == 0:
        return 'Not Subscribed'
    else:
        return 'Subscribed'

df['Newsletter'] = df['Newsletter'].apply(Newsletter)
```
- Converts `0` to `"Not Subscribed"` and `1` to `"Subscribed"`.

#### **Voucher Usage Mapping**
```python
def voucher(e):
    if e == 0:
        return 'Not used'
    else:
        return 'Used'

df['Voucher'] = df['Voucher'].apply(voucher)
```
- Converts `0` to `"Not used"` and `1` to `"Used"`.

#### **Age Group Mapping**
```python
def agegroup(f):
    if f >= 0 and f <= 10:
        return '0-10'
    elif f >= 11 and f <= 20:
        return '11-20'
    elif f >= 21 and f <= 30:
        return '21-30'
    elif f >= 31 and f <= 40:
        return '31-40'
    elif f >= 41 and f <= 50:
        return '41-50'
    elif f >= 51 and f <= 60:
        return '51-60'
    elif f >= 61 and f <= 70:
        return '61-70'
    elif f >= 71 and f <= 80:
        return '71-80'
    elif f >= 81 and f <= 90:
        return '81-90'
    elif f >= 91 and f <= 100:
        return '91-100'
    else:
        return '>100'

df['Age'] = df['Age'].apply(agegroup)
```
- Groups ages into specific ranges.

---

### **4. Analyzing Data**
#### **Q1: What age group buys the most?**
```python
counts = df['Age'].value_counts()
print(counts)

labels = ['41-50', '21-30', '31-40', '51-60', '11-20', '61-70']
plt.pie(counts, labels=labels, autopct='%.1f%%', shadow=True)
plt.show()
plt.savefig('Age group buy from us')
```
- Counts purchases per age group.
- Creates a pie chart to visualize the distribution.

---

#### **Q2: What payment method is used most by age groups?**
```python
df = df.loc[df['Pay_Method'] == 'Card']
ndf = df.groupby(['Age', 'Pay_Method']).agg(t=('Pay_Method', 'count')).reset_index()
ndf = ndf.sort_values(by=['Age', 'Pay_Method'], ascending=[0, 1])
print(ndf)
```
- Filters dataset to include only `Card` transactions.
- Groups by `Age` and counts occurrences.

```python
plt.figure(figsize=(10, 5))
plt.bar(ndf['Age'], ndf['t'], color='darkgreen')

plt.xlabel('Age')
plt.ylabel('Count of Transactions')
plt.title('Number of Card Transactions by Age')
plt.xticks(rotation=45)
plt.show()
plt.savefig('payment method is used most by age groups')
```
- Creates a bar chart to show `Card` usage by age group.

---

#### **Q3: What browsers do our customers use the most?**
```python
df = df.loc[df['Browser'] == 'Chrome']
mdf = df.groupby(['Age', 'Browser']).agg(b=('Browser', 'count'))
mdf = mdf.rename(columns={'b': 'use'})
mdf = mdf.reset_index()
mdf = mdf.sort_values(by='use', ascending=False)
print(mdf)
```
- Filters dataset for `Chrome` users.
- Groups by `Age` to count how many used `Chrome`.

```python
plt.bar(mdf['Age'], mdf['use'], color='green')

plt.xlabel('Age')
plt.ylabel('Count of Browser')
plt.title('Number of Browser used by Age group')
plt.xticks(rotation=45)
plt.show()
plt.savefig('Browsers our customers use most')
```
- Creates a bar chart of browser usage by age group.

---

#### **Q4: Best Month for Sales**
```python
df['Purchase_DATE'] = pd.to_datetime(df['Purchase_DATE'])
df['month_name'] = df['Purchase_DATE'].dt.month_name()
df['month'] = df['Purchase_DATE'].dt.month_name  # Redundant line
df['year'] = df['Purchase_DATE'].dt.year
print(df[['month', 'year']])
```
- Converts `Purchase_DATE` column to a DateTime format.
- Extracts month and year.

```python
df['sales'] = df['Purchase_VALUE'] * df['N_Purchases']
pdf = df.groupby('month_name').agg(totalsale=('sales', 'sum'))
pdf = pdf.reset_index()
pdf = pdf.sort_values(by=['month_name', 'totalsale'], ascending=[1, 0])
print(pdf)
```
- Calculates total sales per month.

```python
plt.figure(figsize=(10, 5))
plt.plot(pdf['month_name'], pdf['totalsale'], marker='o', linestyle='-', color='g')

plt.xlabel('Month')
plt.ylabel('Total Sales')
plt.title('Sales Trend Over Months')
plt.xticks(rotation=45)
plt.grid(True)
plt.show()
plt.savefig('Best month sale')
```
- Creates a line plot of total sales per month.

---

#### **5. Analyzing Gender-Based Metrics**
##### **Average Time Spent by Gender**
```python
jdf = df.groupby('Gender').agg(Timespent=('Time_Spent', 'mean'))
jdf = jdf.reset_index()
jdf = jdf.sort_values(by=['Timespent'], ascending=False)
print(jdf)
```
- Calculates average time spent per gender.

```python
plt.figure(figsize=(8, 5))
plt.bar(jdf['Gender'], jdf['Timespent'], color=['darkgreen', 'greenyellow'])

plt.xlabel('Gender')
plt.ylabel('Average Time Spent')
plt.title('Average Time Spent by Gender')

plt.show()
plt.savefig('Average Time Spent by Gender')
```
- Creates a bar chart of time spent by gender.

##### **Total Revenue and Purchases by Gender**
```python
kjh = df.groupby('Gender').agg(total_revenue=('Revenue_Total', 'sum'),
                               no_of_purchase=('N_Purchases', 'mean')).reset_index()
print(kjh)
```
- Computes total revenue and average purchases per gender.

```python
fig, axes = plt.subplots(1, 2, figsize=(12, 5))

axes[0].bar(kjh['Gender'], kjh['total_revenue'], color=['darkgreen', 'lightgreen'])
axes[0].set_title('Total Revenue by Gender')

axes[1].bar(kjh['Gender'], kjh['no_of_purchase'], color=['forestgreen', 'yellowgreen'])
axes[1].set_title('Avg. No. of Purchases by Gender')

plt.tight_layout()
plt.show()
plt.savefig('Total Revenue by Gender')
```
- Creates bar charts comparing revenue and purchases by gender.

---

