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

#### Step 2: Statistical Exploration + Preparing data
To analyze the dataset and verify that the reading stage was completed successfully, we can examine the dataframe using the .info() method.
![image](https://github.com/user-attachments/assets/9eeb5ca1-2eff-49d5-bce0-738ac928227c)

There are null values in the dataset, indicating a cleaning task.
To drop null values, we would utilize .dropna() method
![image](https://github.com/user-attachments/assets/4af88d23-e516-4b8c-a475-26ee7ccf5d7e)

The dataframe is now clean, allowing us to explore some fundamental statistics using the .describe() method.
![image](https://github.com/user-attachments/assets/ddd7a433-0a64-4dc1-9f77-322348c49e15)
It is incorrect for the minimum quantity to be less than zero, which suggests a data manipulation task. For every quantity value that is less than zero, I would convert them into "0" using this command: ecom_df.loc[ecom_df['Quantity'] < 0, 'Quantity'] = 0.
![image](https://github.com/user-attachments/assets/06b1ce8c-5bd8-44ae-bba4-afb0d7d85d1b)
**Quantity column analysis**

**Statistics:**

- Count: 406,829 entries, indicating the number of transactions recorded for which quantities were specified.
- Mean: Approximately 12.74 items per transaction, suggesting average customer purchases are moderately high per order.
- Standard Deviation: 178.44, indicating significant variability in quantities bought across transactions. Many may skew towards purchases of large quantities.
- Minimum: 0, indicating some transactions had no purchases, perhaps representing returns or empty orders.
- Maximum: 80,995, indicating the potential for bulk orders or anomalous data points that should be further investigated.
**Percentiles:**
- 25th Percentile (Q1): 2—indicating that 25% of transactions involve 2 items or fewer.
- 50th Percentile (Median): 5—indicating that half of the transactions involve 5 items or fewer, suggesting that many transactions are smaller purchases.
- 75th Percentile (Q3): 12—indicating that 75% of transactions involve 12 items or fewer.
**Insights:**
- Typical Purchases: Most customers tend to buy a small quantity of items, with half purchasing 5 or fewer, while a smaller segment leads towards larger transactions.
- Outliers: The extremely high max of 80,995 points to the presence of significant outliers, which could skew average calculations and should be further analyzed.
- Returns/Cancellations: Zero quantities in the dataset will necessitate a review to understand if these represent cancellations, adjustments, or some other factor.
=> Insights from quantity distributions can guide inventory stocking, ensuring that commonly purchased quantities are readily available while also preparing for any potential bulk order management.

**UnitPrice column analysis**

**Statistics:**
- Count: 406,829 entries, indicating the number of transactions with specified unit prices.
- Mean: Approximately 3.46, suggesting that on average, items are sold for about $3.46 per unit.
- Standard Deviation: 69.32, indicating significant variability in unit prices. This suggests that while many items are priced lower, there are some high-priced items affecting the overall average.
- Minimum: 0.00, indicating some items may have been sold for free, likely representing promotions, samples, or errors in the data.
- Maximum: 38,970.00, indicating a significant outlier—likely high-value items, bulk pricing, or incorrect entries.
**Percentiles:**
- 25th Percentile (Q1): 1.25—indicating that 25% of unit prices are $1.25 or lower.
- 50th Percentile (Median): 1.95—half of the items are priced at $1.95 or less.
- 75th Percentile (Q3): 3.75—indicating that 75% of items are priced at $3.75 or lower.
**Insights:**
- Typical Pricing: Most items are priced low, with half of the items costing $1.95 or less, suggesting a focus on affordable products or everyday items.
- Outliers: The extremely high maximum price of $38,970 indicates the presence of significant outliers, which could represent luxury items, errors, or bulk pricing strategies. This could skew average price calculations, making it crucial to examine these transactions.
- Free Items: The presence of a minimum price of $0.00 indicates some transactions might involve free items, potentially for promotions or marketing strategies.
- Pricing Strategy: Insights into pricing can inform marketing tactics, such as bundling lower-priced items.

For categorical columns such as Stock Code and Country, I am interested in determining the number of unique products sold and the count of customers in each country.
![image](https://github.com/user-attachments/assets/c3ab8e90-7f94-4ee1-93cb-1d149ca79eeb)

Now, let's visualize the distribution of these key metrics to gain a clearer understanding of them.

#### Step 3: Ditribution Visualization
As for the key metrics of `Quantity` and `UnitPrice`, we can:
![image](https://github.com/user-attachments/assets/3d7b9cc0-f917-486c-9a77-58e885f41cf3)
![image](https://github.com/user-attachments/assets/e482edf5-7503-40ac-b45b-e01bc2f3d2ba)


As illustrated, the outliers are distorting the distribution, making it challenging to identify normal distributions. While we must remain aware that these outliers could provide valuable insights, we should first examine the normal distribution.
![image](https://github.com/user-attachments/assets/819155e2-6dc1-4cc9-9f20-8ae83bb8ac4b)
![image](https://github.com/user-attachments/assets/5f3c2bdb-2857-421b-a33a-edc7ec32a5d5)

!!! Analyze this

As for categorical variables - `Country`:
![image](https://github.com/user-attachments/assets/dfcfe364-0352-4bb8-b423-cc14e42e3f8a)





