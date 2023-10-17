# Electronics
Python

## Summary

I've conducted a comprehensive analysis of the electronics sales data. Key insights include the identification of top-selling products, the revelation of peak sales hours and days for targeted marketing, and a nuanced understanding of sales trends over time, which will inform seasonal stock planning. Additionally, I've delved into the price-sales relationship and discovered cross-selling opportunities, providing actionable insights to enhance our business strategies and maximize sales in the electronics market.

## Numeric summary
numeric_summary = sales_data.describe()
print(numeric_summary)

- The dataset includes 185,950 orders with a wide range of order IDs, product EANs, and quantities ordered.
- Pricing and margin data reveals variations, with product prices per unit ranging from a minimum of $1.00 to a maximum of $1,700, and cost prices ranging from $2.99 to $1,700. Margins, calculated as the difference between turnover and cost price, vary from $1.50 to $2,278.
- The dataset has a mean order date of July 18, 2019, and it spans from January 1, 2019, to January 1, 2020. Understanding the distribution of these values and their impact on overall turnover and profitability is crucial for further analysis.

## Top selling products

![image](https://github.com/beishenov3197/electronics/assets/112967670/ed63084a-b389-4184-bc40-b88a3b3663f7)

top_selling_products = sales_data.groupby('Product')['Quantity Ordered'].sum()
print("Top-Selling Products and Their Quantities:")
print(top_selling_products)

- The top-selling products ('AAA Batteries (4-pack)' and 'AA Batteries (4-pack)') present opportunities for bundled offerings and loyalty programs, given their high sales volume.
- High-tech items like the '27in 4K Gaming Monitor' and 'Apple Airpods Headphones' highlight strong demand for premium tech experiences, offering potential for targeted marketing.
- Niche products like the 'LG Dryer' and 'LG Washing Machine' provide insights into the home appliance market, helping identify trends and growth opportunities in this segment.

## Peak hours of sales

![image](https://github.com/beishenov3197/electronics/assets/112967670/4eff7531-5fa7-471b-9bec-37e917870776)

sales_data['Order Date'] = pd.to_datetime(sales_data['Order Date'], errors='coerce')
sales_data['Order Hour'] = sales_data['Order Date'].dt.hour
peak_sales_hours = sales_data.groupby('Order Hour')['Quantity Ordered'].sum()
peak_hour = peak_sales_hours.idxmax()
print("Peak Sales Hours:")
print(peak_sales_hours)
print(f"The peak sales hour is at {peak_hour} o'clock.")
peak_sales_hours.to_csv("Peak_hour.csv")

- Sales exhibit a morning peak, with the highest sales occurring at 9 am, suggesting a strong demand in the early hours.
- There is a midday dip in sales, highlighting a potential opportunity for targeted lunchtime promotions.
- Sales surge again in the evening, particularly from 6 pm to 8 pm, offering an opportunity for businesses to focus their marketing efforts during this peak period.

## Peak Day

![image](https://github.com/beishenov3197/electronics/assets/112967670/b117f706-4087-4e7a-bb81-ed4800f6ed3c)

sales_data['Order Day of Week'] = sales_data['Order Date'].dt.dayofweek
peak_sales_days = sales_data.groupby('Order Day of Week')['Quantity Ordered'].sum()
peak_day = peak_sales_days.idxmax()
print("Peak Sales Days:")
print(peak_sales_days)
print(f"The peak sales day is on {peak_day} (0=Monday, 1=Tuesday, ..., 6=Sunday).")
peak_sales_days.to_csv("Peak_day.csv"


- The data indicates that Tuesday (1) is the peak sales day, with 30,724 units ordered, suggesting that midweek promotions and marketing efforts can be particularly effective in maximizing revenue.
- While Tuesday is the leader, other weekdays also maintain relatively high sales volumes, highlighting the need to strategically plan inventory and promotional campaigns throughout the week to ensure consistent sales performance.
- Despite midweek dominance, weekends (Saturday and Sunday) demonstrate notable demand, emphasizing the importance of weekend-specific marketing strategies to capture sales opportunities and ensure a well-rounded approach across the entire week.

## Sales trends over time

![Monthly_Sales_Trend](https://github.com/beishenov3197/electronics/assets/112967670/aefe7948-14b4-4411-b1b4-7d9b7211ebd4)

sales_data['Order Date'] = pd.to_datetime(sales_data['Order Date'], errors='coerce')
monthly_sales = sales_data.resample('M', on='Order Date')['turnover'].sum()
plt.figure(figsize=(12, 6))
plt.plot(monthly_sales.index, monthly_sales.values, marker='o', linestyle='-')
plt.xlabel('Month')
plt.ylabel('Total Sales')
plt.title('Monthly Sales Trends')
plt.grid(True)
plt.show()

- Data shows sales growth until May, a strong season for business.
- Sales decrease from early summer to autumn, needing strategies to boost engagement.
- A revival in sales from September to year-end, followed by a sharp drop, suggests opportunities for targeted marketing and seasonal stock planning.

## Relationship between product price and sales

![Relationship_between_product_price_and_sales](https://github.com/beishenov3197/electronics/assets/112967670/f80ed6e7-814c-4c91-9f41-2569fa77028d)

product_price = sales_data['Price Each']
sales = sales_data['turnover']
plt.figure(figsize=(8, 6))
plt.scatter(product_price, sales, alpha=0.5)
plt.xlabel('Product Price')
plt.ylabel('Sales (Turnover)')
plt.title('Relationship between Product Price and Sales')
plt.grid(True)
correlation = np.corrcoef(product_price, sales)[0, 1]
print(f'Correlation coefficient: {correlation:.2f}')
plt.show()

- Concentration in the mass market category
- A limited presence in the mid-range segment
- A minor presence beyond the specified spending thresholds
  
## Cross-selling opportunities

![image](https://github.com/beishenov3197/electronics/assets/112967670/612e0cb7-ccee-4901-b3fc-08b2d91dc891)

sales_data = pd.DataFrame(data, columns=columns)
basket = (sales_data
          .groupby(['Categorie', 'Product'])['Product']
          .count().unstack().reset_index().fillna(0)
          .set_index('Categorie'))
frequent_itemsets = apriori(basket, min_support=0.05, use_colnames=True)
rules = association_rules(frequent_itemsets, metric="lift", min_threshold=0.5)
sorted_rules = rules.sort_values(by='lift', ascending=False)
print(sorted_rules)
sorted_rules.to_csv("Cross-selling.csv")

- The data shows two rules: one where people who bought an 'iPhone' also bought 'Wired Headphones,' and the other where people who bought 'Wired Headphones' also bought an 'iPhone,' suggesting a strong connection between these products.
