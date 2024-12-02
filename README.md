# python_rfm 
Python - Superstore - Using RFM analysis to better promotional campaign

## Context Overview
Business Context
SuperStore: A global online retailer facing challenges in understanding and segmenting its customer base.
Marketing Goals:
- Understand purchasing behavior to improve retention strategies through promotional campaigns.
- Differentiate between loyal and one-time customers to optimize marketing campaigns.

## Thinking process
Let's decompose and analyze the problem and available data
  Dataset link - Dataset includes:
ecommerce retail table:
  - InvoiceNo: Invoice number. Nominal, a 6-digit integral number uniquely assigned to each transaction. If this code starts with letter 'C', it indicates a cancellation.
  - StockCode: Product (item) code. Nominal, a 5-digit integral number uniquely assigned to each distinct product.
  - Description: Product (item) name. Nominal.
  - Quantity: The quantities of each product (item) per transaction. Numeric.
  - InvoiceDate: Invoice Date and time. Numeric, the day and time when each transaction was generated.
  - UnitPrice: Unit price. Numeric, Product price per unit in sterling.
  - CustomerID: Customer number. Nominal, a 5-digit integral number uniquely assigned to each customer.
  - Country: Country name. Nominal, the name of the country where each customer resides.
=> To understand the set more clearly, an EDA analysis would be conducted.
- Need for Insight: The marketing team lacks insights into customer segments that inform personalized marketing strategies. This implies the need to label customers into different segments based on their behaviors, such as the last purchase, how frequently they purchase from us, and how much they have earned Superstore. To do so, we would need a systematic way to segment customers - RFM analysis. An RFM scoring table would be built to rank our customers.
RFM table
![image](https://github.com/user-attachments/assets/76e64b69-61e9-4555-966b-b4e41b056712)

## Action
### Stage 1: Conducting Exploratory Data Analysis
#### Step 1: Installing libraries
Installing universally necessary libraries such as pandas, numpy, seaborn, matplotlib. Also, openpyxl was installed to help read the Excel file better.
![image](https://github.com/user-attachments/assets/391757fe-39e3-4e7c-8c14-39b297d61429)

#### Step 2: Exploring
![image](https://github.com/user-attachments/assets/9eeb5ca1-2eff-49d5-bce0-738ac928227c)

There are null values in the dataset, indicating a cleaning task.




