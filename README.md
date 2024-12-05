# Python | Retail Sector | How RFM Analysis Unlocked Insights to Drive Engagement and Maximize Revenue

## Context Overview
Business Context
Superstore, like many ecommerce businesses, struggles to effectively retain its most valuable customers and maximize revenue. With an increasing volume of transactional data, understanding customer behavior is critical for segmentation, targeted marketing, and loyalty-building initiatives. T
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

Critical question to answer for this project: How can Superstore effectively leverage RFM segmentation to foster stronger customer relationships, re-engage at-risk segments, and gain actionable insights into customer behaviors that ensure profit maximization and long-term retention?
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

It is not valid for the minimum quantity to be less than zero, indicating the need for data manipulation. Any rows containing quantity values below zero will be removed. Converting these negative values to zero would artificially inflate the frequency, leading to potentially misleading insights.

![image](https://github.com/user-attachments/assets/d649c097-1c1d-4ce4-8111-c8fe25b4ede2)

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

Customers typically purchase baskets containing either 2 to 5 items or approximately 10 to 15 items.

![image](https://github.com/user-attachments/assets/5f3c2bdb-2857-421b-a33a-edc7ec32a5d5)

The preferred price points for customers typically range from $1 to $2, while the overall distribution of prices spans from just above $0 to $7. This indicates that most purchases are concentrated at lower price levels, with a small portion extending towards the higher end of the spectrum.

To address the anomalies, I will organize those transactions into a dataframe. This will allow us to identify the customer segments associated with these outlier transactions and analyze their behaviors in order to gain deeper insights.

![image](https://github.com/user-attachments/assets/02d7492b-a9ca-4b2d-a957-6bd26a1c281f)

Before moving on, a column of `Revenue` would be added to the original dataframe of ecom_df to confirm what we've known.

![image](https://github.com/user-attachments/assets/0faf6718-eeae-488c-818b-0de157cb3411)

**Statistics**
- Mean: The average revenue per transaction is approximately $21.90. This value provides a baseline for understanding typical sales amounts.
- Standard Deviation (std): quite high relative to the mean, indicating significant variability in revenue. This suggests that while many transactions hover around the mean, there are numerous transactions (especially high-revenue transactions) that contribute to this high variability.
- Minimum: The minimum revenue is $0.00, which likely indicates transactions where no items were purchased, returns, or other anomalies. It's important to analyze the context of these zero-revenue transactions, as they could skew statistics and insights.
- 25th Percentile (Q1): 4.20 - This means that 25% of the transactions generate revenues of $4.20 or less. It's indicative of lower-value transactions, possibly from bulk purchases or lower-priced items.
- 50th Percentile (Median, Q2): 11.10 - The median revenue is $11.10, which tells us that half of the transactions produce revenue equal to or less than this amount. The median is less influenced by high outliers, offering a better center point of typical transaction values.
- 75th Percentile (Q3): 19.50 - This shows that 75% of the transactions generate revenues of $19.50 or less. It has a significant implication on upper transaction value trends.
- Maximum: 168,469.60 - The maximum revenue is notably high, indicating that there are very high-value transactions that can skew average calculations (mean) and may represent either bulk orders or high-value customer purchases.

**Insights**

- High Variability: The high standard deviation compared to the mean indicates a wide range of revenue amounts. Many transactions likely fall below the average, but a few high-revenue transactions significantly impact the overall mean.
- Low Revenue Transactions: With a minimum of zero and a first quartile of $4.20, many transactions likely involve low sales amounts. This could reflect sales of inexpensive items or possible returns.
- Transactional Insights: The difference between the 25th percentile ($4.20) and the median ($11.10) indicates that while there are low-value transactions, the median revenue rises significantly, showing that many customers are also purchasing more expensive items.

This histogram (outliers removed) will confirm the insights provided: 

![image](https://github.com/user-attachments/assets/46c4d1a2-19f4-4744-9713-229c285a112c)


As for categorical variables - `Country`:

![image](https://github.com/user-attachments/assets/8cea9d4b-40f8-49c0-83a4-6910482eb3ba)

### Stage 2: RFM Calculation and Exploration
#### Step 1: Create a dataframe of unique customers
As RFM is a customer-level metric (we label customers one by one), a unique customer dataframe is needed to proceed the next steps.

![image](https://github.com/user-attachments/assets/193b94cd-4441-482e-adb5-ab34ed99eea4)

#### Step 2: Calculate Customer Recency
Recency = the nearest date that the customers engaged with us through purchases => 

![image](https://github.com/user-attachments/assets/44fd78dc-e659-46be-ab69-08b4850bc4ba)
Renaming the column to match the context: 

![image](https://github.com/user-attachments/assets/6f9587e7-2eb3-4338-b45e-1a96e873fc05)

Given the context of the dataset's date, I will treat "2012-01-01" as the current date. We will proceed to create a "recency" column that calculates the number of days from "recency_date" to "2012-01-01". This duration is referred to as the timedelta between the analysis date and the "recency_date." The resulting time difference will then be divided by a timedelta representing one day to convert it into a numerical value that indicates recency in days. Below is an explanation of how the code functions:

![image](https://github.com/user-attachments/assets/101a9a33-da6e-4d15-8d83-057f9042934f)


#### Step 3: Calculate Customer Frequency

Frequency refers to the number of times customers have made purchases, determined by counting unique invoices. The subsequent code operates as follows: it calculates the "frequency" value by counting the unique invoices and grouping the results by CustomerID. Then, the column name "InvoiceNo" is changed to "frequency." Finally, this new frequency column is merged into the previously created ecom_customer dataframe.

![image](https://github.com/user-attachments/assets/01531dd5-e707-49dc-a598-6e5b82782c06)

#### Step 4: Calculate Customer Monetary
Monetary value essentially represents revenue, which we have established previously. Our next step is to rename the column to "monetary" and incorporate it into the ecom_customer dataframe. Here’s the process:

![image](https://github.com/user-attachments/assets/2d64f5f3-0d25-48e5-b811-0efca6d48083)

##### Step 5: Examine the statistical distributions

![image](https://github.com/user-attachments/assets/44eaad5f-a584-49f8-aabb-fca50c314c25)

![image](https://github.com/user-attachments/assets/439b3c65-e33a-455c-8796-c041f61d88f8)

Recency comment:
- Seems like there is a significant number of customers engaging with Superstore e-commerce (under 100 days).
- These customers are quite active, indicating that they might be more responsive to Superstore marketing campaigns, indicating the potential targets.
- The values distributed in other bins suggest that Superstore need to re-engage customers with >100 days in recency.

Frequency comment:
- The concentration of density and bars in the 0-50 range suggests that there might be a segment of customers who are more active and engaged with Superstore's e-commerce platform. These customers with higher frequencies may represent a key target segment for the company's marketing efforts and retention strategies.
- For those in the tail of the analysis, we might need to analyze them to understand their behaviors and unique needs as high frequency customers do bring significant values.

Monetary Comment
- This is also a skewed distribution, meaning that only a few customers generate substantial monetary value to Superstore. This indicates a deeper investigation to reveal the high-value customers, who need special treatments.
- This distribution indicates the spending habit of the current customers of Superstore, focusing on small transactions. This insight can help in tailoring the company's marketing and pricing strategies.

Now, I would like to examine the normal distribution of the metrics, excluding outliers, to gain a clearer understanding of typical behaviors.

![image](https://github.com/user-attachments/assets/ce1cb380-86e8-4055-9129-be90f2ab5842)
![image](https://github.com/user-attachments/assets/f2c0b5e9-a84c-475d-93ca-d86552a742e7)

Recency comment:
- As observed earlier, Superstore's customers are quite active, who tend to come back to the store in around a month or so. Knowing that 25 days is a common recency value can guide your marketing timing for promotions, reminders, or campaigns to maximize conversion rates. For example, sending an email reminder or promotional offer could be timed for 20-25 days after their last purchase.

Frequency comment:
- However, most customers are one-time customers, with some of them do return twice or more. The business could focus on ways to convert these one-time buyers into repeat customers, such as follow-up emails, loyalty programs, or targeted promotions.

Monetary Comment
- The mode of around $200 suggest that many transactions occur at or around this value, which could suggest that $200 is a typical price point for popular products or services offered by the business. The business might identify segments of customers who tend to spend around $200, allowing for targeted marketing strategies and personalized offers to encourage those customers to increase their spending or shop for complementary products.

### Stage 3: Calculating RFM scores using quantile-based discretization
#### Step 1: Calculate the R score

Since customers with lower recency are considered more valuable than those with higher recency, the scoring methodology for the "R" category differs from the others. To divide customers into approximately equal groups, I will use the qcut() function on the recency column and assign labels ranging from 5 to 1. This ensures that the highest score (5) is assigned to customers with the lowest recency, indicating their higher value.


![image](https://github.com/user-attachments/assets/74231599-c4d6-40fc-b681-70959ee4d3e5)


Let's take a look at the scores to briefly understand our customers: 

![image](https://github.com/user-attachments/assets/0ebd3bb1-c84a-4dd0-ac34-98513c7ef7a9)

This table provides clear insights into customer behavior based on their labels. For instance, customers labeled as '5' typically return to Superstore after an average of approximately one month and 27 days. In contrast, those labeled as '1' do not appear to return even six months after their initial purchase.

#### Step 2: Calculate the F score
While calculating the F score, I discovered that data skewness presents a significant challenge due to overlapping bin edges, resulting in ambiguity when assigning data points.

Explanation:

Quantile-based discretization - qcut() - divides the dataset into bins based on percentiles of the overall data distribution, without considering the specific distribution or pattern within the data, such as the skewed distribution of order volumes. This can result in closer bin edges in regions with more concentrated data points, leading to overlapping bins. As a consequence, some data points may fall into multiple bins simultaneously, causing ambiguity in their assignment.

For instance, let's say the first bin edge is at 50, and the second bin edge is at 100. If we have two order volumes, 55 and 60, they are relatively close to each other. However, due to the overlapping bin edges, one of these values may be assigned to the lower bin, while the other falls into the higher bin, causing ambiguity and potential misclassification.

Solution: Using .rank(method = 'first')

This method assigns a unique position or rank to each value based on their order within the data. This guarantees that each value is distinct and eliminates the possibility of overlapping bin edges.

![image](https://github.com/user-attachments/assets/1f0f6247-1a97-4fe3-bd33-bc6a7d860a07)
![image](https://github.com/user-attachments/assets/80d8e3d5-9ba5-4fb0-b280-17ebea3a27ab)

#### Step 3: Calculate the M score
Moving forward, the scale will range from 1 to 5, with 5 representing the highest value generated.

![image](https://github.com/user-attachments/assets/b8c97e31-e163-49a9-a663-50c4fed106b1)

### Step 4: Creating the RFM segment column and RFM score column

![image](https://github.com/user-attachments/assets/9291cbf4-49dc-4fcc-98b6-085406826816)

In order to quickly sort and rank the customers, a RFM score is needed, which can also give us a better overall asessment and comparison.

![image](https://github.com/user-attachments/assets/467b4e01-032b-4084-89a4-8f2de1902bab)

Let’s take a closer look at the mean scores for each category.

![image](https://github.com/user-attachments/assets/f9d7ac07-14bf-428e-952b-3957212e46db)

This figure provides us with a more comprehensive understanding of Superstore's customers. By examining the customers with the lowest rfm_score of 3, we observe that they have an average recency of 307 days, place a single order, and generate approximately 139 dollars in revenue. In contrast, the highest scorers in rfm_score indicate that they engaged with Superstore less than a month ago, made more than 22 orders, and generated over 11,000 dollars in revenue. On top of that, the table allows Superstore's marketers to see who to target with reactivation campaigns or who should be given special treatment to have them continue being the champions.

### Step 5: Joining data (seg_df)
Now, we merge with the seg_df to label our customers with a name.

![image](https://github.com/user-attachments/assets/99e51bc0-fa22-4ba4-a8f1-4872a5c76de9)


Making sure the rfm_value is the correct data type (we don't want to perform any aggregation on it)

![image](https://github.com/user-attachments/assets/24dfcaf3-c0b8-4d3a-ae2b-b8f889cf48c9)

Merge the column with our customer_rfm_df created earlier on `rfm_value`
![image](https://github.com/user-attachments/assets/cc51b80b-cbed-431b-9042-5297d34d3f9c)

We are now ready to visualize and analyze the result.
![image](https://github.com/user-attachments/assets/c7cd16ee-cdc0-4fc8-b8ad-732590137f4e)

### Stage 4: RFM Visualization
To illustrate RFM, the best visualization would be tree map.
![image](https://github.com/user-attachments/assets/84e58a00-4087-4402-ae63-af146f6a2b36)

Setting up colors and construct `segment_count`, which is used to count each segment's number.

![image](https://github.com/user-attachments/assets/1b2de516-7946-4ec3-ba80-e1decd089978)

Merging customer_rfm_df and ecom_df to facilitate the visualization and future aggregation if need be.

![image](https://github.com/user-attachments/assets/874f7ee2-be16-4cc5-8868-cd903723d7c8)

Visualizing the RFM tree map based on sales and customer number
Segment by customer: Top segments include 
- Champion
- Hibernating
- Potential Loyalist
- Loyal
- Need attention
- At risk
- Lost customers
- 
![image](https://github.com/user-attachments/assets/0bcfee59-b1f1-426d-9a51-028bea3ca850)

Segment by sales: Top segments include 
- Champion
- Loyal
- At risk
- Potential Loyalist
- Need attention
- Hibernating customers
- Cannot lose them
  
![image](https://github.com/user-attachments/assets/493b8a55-1583-474e-9043-fb4c487ef8bf)

Based on our analysis, we should focus our efforts primarily on the following customer segments: Champions, Loyal customers, At-risk customers, Potential Loyalists, Customers Needing Attention, and Hibernating customers. If we have additional resources, we should also prioritize managing the Cannot Lose, Promising, and New customers. Meanwhile, we can deprioritize our efforts on Lost Customers, as they do not contribute significantly to our overall value.

To gain insight into the behaviors of these segments, let’s examine their actions through the lens of score distribution.

![image](https://github.com/user-attachments/assets/04b4b94a-8bdb-4137-a43c-a9097782d1c8)
![image](https://github.com/user-attachments/assets/13b87d39-180b-4cd4-bd99-59d283033e29)


These two figures provide valuable insights into the behaviors of each segment, enabling us to determine the best strategies for targeting them. For instance, if we see that the scores for the Loyal and Champion segments are 13, 14, and 15, we can refer to the table to analyze their specific recency, frequency, and monetary values. Armed with this information, we can design personalized marketing campaigns that recur every month, featuring appropriately bundled items valued between $2600 and $10000.


The column chart above displays the relationship between segments and their corresponding RFM scores. The bar charts illustrate the performance of each score (3 to 15) in terms of recency, frequency, and monetary values.

- Champions: They bought most recently (under 2 months), most often (7-20 times), and are heavy spenders (more than 4000 dollars) (Score 13-15).
- Loyal Customers: They are consistent and frequent shoppers who have made recent purchases and spent a significant amount (Score 11-13).
- Potential Loyalists: They are recent customers (2-4 months) with average frequency (under 10 times) and who spent a good amount (between 1000 to 4000 dollars) (Score 8-13).
- At Risk Customers: They purchased often and spent big amounts but haven't made a purchase recently. Their mean recency ranges from 2 to 4 months (Score 7-12).
- Need Attention Customers: They were once valuable customers but have become less engaged over time. Currently, they are active, purchasing quite frequently, and generating a good amount of revenue (Score 9-13).
- Promising Customers: They show potential for becoming loyal and valuable to Superstore. They have moderate RFM scores, indicating recent engagement (2-4 months ago), average frequency (under 5 times), and a moderate spending amount (under 2000 dollars) (Score 7-10).
- New Customers: Their most recent engagement was roughly 6 months ago. They have a moderate overall RFM score, infrequent purchases (under 5 times), and generate a subtle monetary impact (under 2000 dollars) (Score 5-8).
- Can't Lose Them: They used to visit and purchase quite often but haven't been visiting recently. They share similar traits with current New Customers (Score 5-8).
- Hibernating Customers: They haven't made a purchase recently (more than 6 months ago) and their past purchasing behavior was infrequent with small spending amounts (Score 5-8).
- About to Sleep: They recently made a purchase but haven't been active or frequent shoppers (Score 5-7).
- Lost Customers: They have low recency and frequency scores, indicating they haven't made a purchase in a long time and were infrequent shoppers (Score 1-4).


Taking a look at the spending pattern of each segment this year:

![image](https://github.com/user-attachments/assets/f302a351-69e0-4241-8e65-c3e16d862e6a)

At first glance, the segment performance breakdown could be analyzed as follows:
Champions:
- Champions are Superstore's best customers, who bought most recently, most often, and are heavy spenders.
- After a year, the revenue generated from these customers skyrocketted, indicating Superstore's success in taking care of them.
- Dominating in terms of customer quantity and revenue generation.
- Considered as breadwinners for the company.
- High-value and highly engaged customers.
Hibernating Customers:
- Substantial customer quantity but generate modest revenue.
- Low engagement and value contribution.
- Hiberanating customers are waking up, potentially on their way transforming into the more preferred segments.
Loyal:
- Dominating segment in terms of customer quantity and revenue generation.
- High-value and highly engaged customers.
- Similar to the Champions, Superstore are giving the loyalists what they really want.
At Risk:
- Considered as breadwinners for the company.
- High customer quantity and revenue generation.
- Substantial revenue generated after a year
- However, there may be some concerns about their future engagement and loyalty.
Need Attention
- Generated great amount of revenue
- With a potential customer base
Potential Loyalists:
- Potential Loyalists are your recent customers with average frequency and who spent a good amount.
- Considered as breadwinners for the company.
- Moderate customer quantity, but they show potential for higher engagement and revenue generation.
Lost Customers:
- Substantial customer quantity but generate insignificant value to the company.
- These are customers who have stopped engaging with the company and may need efforts to regain their interest.
Cannot Lose Them:
- The impact of this segment is equivalent to their numbers.
- High-value customers who are crucial for the company's success.
New and Promising:
- Currently generating subtle impact to the business.
- The number of new customers is moderate, but they have the potential to become more valuable over time.
About To Sleep:
- Substantial customer quantity but generate insignificant value to the company.
- These customers may be at risk of disengagement and require attention to prevent them from becoming lost customers.
Based on this analysis, the company should focus on maximizing the potential of the breadwinners (Champions, At Risk, Loyal, Need Attention, and Potential Loyalist) while re-engaging with the Hibernating customers. Additionally, efforts can be made to nurture the New and Promising segment to increase their impact on the business.

We will gain insights into our customers' performance based on each type of RFM score. In this step, I will tally the number of customers assigned to each specific RFM score to assess the company’s overall performance.

![image](https://github.com/user-attachments/assets/917d2451-abe8-4ffa-83d2-0c5e7ad01dd4)

In general, Superstore is performing well in terms of its most impactful RFM score - recency, as the majority of their customers fall into the active range. This suggests that the company is successful in both acquiring and retaining its customers.

When considering the F and R scores, which exhibit skewed distributions, we can observe that a large portion of customers have scores of 4 or 5. This indicates that, overall, the company is performing exceptionally well.


## Recommendation
**Champions: **
This segment consists of customers who have made recent purchases, are frequent shoppers, and are heavy spenders and they are performing exceptionally well. Prioritizing this segment can have a significant impact on Superstore's revenue during the Christmas season.
- Offer exclusive early access to Christmas sales or promotions, allowing them to shop before the general public.
- Provide them with special discounts or coupons to thank them for their loyalty and encourage them to make additional purchases.
- Consider organizing an exclusive event or VIP shopping experience for Champions, where they can enjoy personalized assistance and special perks.
- Doing so can lead to increased spending, repeat purchases, positive word-of-mouth, and potentially attracting new customers who aspire to be part of this esteemed segment. Prioritizing Champions can contribute to driving substantial sales growth and building a strong customer base.

**Loyal Customers:**
This segment has consistent and frequent shoppers who have made recent purchases and spent a significant amount. They are doing thriving, yet not as much as the Champions. Our goal is to convert them gradually into Champions.
- Send personalized thank-you notes expressing gratitude for their continued support and loyalty. Offer them exclusive discounts or rewards as a token of appreciation for their frequent visits and purchases. Provide recommendations for Christmas gift ideas based on their past purchases, helping them find the perfect gifts for their loved ones.
- Doing so can help Superstore:
  - Strengthen customer-brand relationships, boosting loyalty and repeat purchases.
  - Harness word-of-mouth marketing as loyal customers become brand advocates and share positive experiences.
  - Maximize customer lifetime value with ongoing purchases and potential upselling opportunities.
  - Foster emotional connections, enhancing the customer experience and brand perception.

**Potential Loyalists:**
By prioritizing potential loyalists, Superstore can increase engagement, convert them into loyal customers, drive revenue growth, boost customer lifetime value, generate positive word-of-mouth, and foster brand loyalty.
- Offer membership or loyalty programs or recommend related products to upsell them and help them become your Loyalists or Champions.
- Invite them to join a loyalty program or membership that offers benefits and rewards during the Christmas season.
- Recommend related products or accessories that complement their previous purchases, suggesting they explore additional items.
- Provide personalized product suggestions based on their preferences and buying history, showcasing the variety of Christmas-related products available.

**At Risk Customers:**
Our At Risk customers are generating good revenue, we should not neglect them in this campaign. Prioritizing At Risk customers can prevent revenue loss, retain their business, uncover upselling opportunities, build loyalty, gather valuable insights, and preserve advocacy potential.
To do:
- Send them personalized reactivation campaigns to reconnect, and offer renewals and helpful products to encourage another purchase.
- Reconnect with personalized Christmas-themed campaigns, reminding them of their past purchases and the value they received.
- Offer them exclusive discounts or promotions to encourage them to make a Christmas purchase and re-engage with the brand.
- Highlight any improvements, new product lines, or special holiday offers to reignite their interest and show that Superstore is committed to their satisfaction.

**Need Attention Customers:**
- Similar to the At Risk customers, Need Attention customers are potential enough for us to invest in. We can:
- Express appreciation for their continued support and recent purchases, thanking them for their loyalty.
- Offer them exclusive promotions or discounts on Christmas-related products to encourage repeat purchases.
- Provide personalized recommendations based on their preferences and past purchases, demonstrating that their needs are understood and catered to.

If we have more resources, invest a bit in these customers:
**Promising Customers:**
We can invest subtly in these customers by:
- Extend a warm thank-you message for their recent engagement and potential as valuable customers.
- Offer them special promotions or discounts on Christmas-related products, encouraging them to explore more and solidify their relationship with the brand.
- Provide personalized product suggestions based on their recent engagement, showcasing relevant Christmas items or limited edition products.
**New Customers:**
These customers are unknown, who might be a Champion customer one day. We should show our hospitality.
- Start building relationships with these customers by providing onboarding support and special Christmas offers to increase their visits.
- Welcome them to Superstore and express gratitude for choosing the brand for their Christmas shopping.
- Offer them a special first-time customer discount or incentive to encourage repeat purchases.
- Provide guidance on Christmas gift ideas or popular products, helping them navigate their holiday shopping experience.

**Can't Lose Them:**
- Taking a good look into our history with these customers to gain insights is wise as we don't want other segments' customers to fall into the danger zone. We should:
- Bring them back with relevant promotions, and run surveys to find out what went wrong and avoid losing them to a competitor.
- Re-engage with personalized messages, reminding them of their past frequent visits and purchases.
- Offer them special incentives, such as loyalty rewards or personalized promotions, to encourage them to return and make a Christmas purchase.
- Request feedback or conduct surveys to understand any concerns or issues they might have, and take steps to address them promptly.

**Hibernating Customers:**
- Rekindle their interest with a heartfelt Christmas-themed message, expressing a desire to reconnect.
- Offer them exclusive discounts or incentives to encourage them to make a Christmas purchase and rediscover the brand's offerings.
- Highlight any improvements or new products/services since their last engagement, showcasing the brand's commitment to providing value.

**About to Sleep:**
- Send a gentle reminder of their recent purchase and express appreciation for their support.
- Offer personalized recommendations or suggestions for Christmas gifts or related items based on their previous purchase.
- Highlight any limited-time offers or promotions to motivate them to make additional purchases during the holiday season.

**Lost Customers:**
- Express appreciation for their past support and extend a genuine invitation to reconnect during the Christmas season.
- Offer a special one-time discount or incentive to encourage them to make a comeback.
- Share updates on improvements, new product lines, or customer-centric initiatives to reignite their interest and show that the brand has evolved.


### Abnormal Transactions Analysis
Now, to address abnormal transactions, we will identify the customers involved in them and consider launching additional campaigns tailored to these customers. We will merge the abnormal transaction data frame we created earlier with the customer RFM data frame.

![image](https://github.com/user-attachments/assets/8368829d-b845-460a-b6d1-776d7a4c530c)


It seems that our targeted segments in the recommendation part are more likely to be involved in abnormal transactions, which is indeed a good signs 
![image](https://github.com/user-attachments/assets/09d2993e-2361-449e-bf1a-2e016ad1e01d)

In terms of personalizing our campaigns, this table will be utilized for deeper analysis and to launch targeted campaigns aimed at maximizing value from these customers.

![image](https://github.com/user-attachments/assets/a985708a-2bc8-456b-814d-45ba57caeac4)




