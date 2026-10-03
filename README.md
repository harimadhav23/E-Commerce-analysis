# E-Commerce Business Analytics – Power BI

## Project Overview

This project analyzes an e-commerce business using **Microsoft Power BI** to turn transactional data into meaningful business insights.

The analysis covers the following business areas:

- Sales Performance
- Customer Analytics
- Product & Category Performance
- Orders & Payment Operations
- Customer Value & Retention
- Executive Summary

A final **Executive Summary Dashboard** brings the most important KPIs and findings together for management-level review.

---

## Business Objective

The main objective of this project is to understand how the e-commerce business is performing across sales, customers, products, and order operations, and to convert the analysis into actionable business insights.

The project follows this BI workflow:

**Raw Data → Power Query → Data Model → DAX → Interactive Dashboards → Business Insights → Recommendations**

---

## Dataset

The project uses a provided e-commerce dataset containing four related tables:

| Table | Purpose | Approx. Rows |
|---|---|---:|
| `customers` | Customer information, location and signup details | 100 |
| `orders` | Order date, status and payment information | 500 |
| `order_items` | Product-level transaction details | 1,500 |
| `products` | Product, category, price and stock information | 50 |

### Key Fields

**customers**
- `customer_id`
- `customer_name`
- `city`
- `country`
- `signup_date`

**orders**
- `order_id`
- `customer_id`
- `order_date`
- `status`
- `payment_method`

**order_items**
- `order_item_id`
- `order_id`
- `product_id`
- `quantity`
- `unit_price`
- `line_sales`

**products**
- `product_id`
- `product_name`
- `category`
- `price`
- `stock`

> The exact external source of the dataset was not specified in the available project materials, so this README refers to it as the provided e-commerce dataset.

---

## Data Cleaning & Transformation

Data preparation was performed in **Power Query**.

Key steps included:

1. Correcting data types for IDs, dates, quantities and numeric fields.
2. Cleaning text fields such as city, category, status and payment method.
3. Creating the `line_sales` calculation:

```text
line_sales = quantity × unit_price
```

4. Checking duplicates and key consistency.
5. Reviewing relationships between customer, order, order-item and product data.
6. Identifying missing item-level records for some delivered orders.

### Data Quality Note

The analysis identified **19 delivered orders without corresponding item-level details**. These records were retained rather than deleted, because the order-level information is still useful for order and operational analysis. Revenue based on item-level transactions should therefore be interpreted with this limitation in mind.

---

## Data Model

The Power BI model uses a relational/star-schema-style structure:

```text
Customers  1 ───── *  Orders  1 ───── *  Order Items  * ───── 1  Products
                         │
                         │
                         *
                         │
                    DateTable  1 ───── * Orders
```

### Relationships

- `customers[customer_id]` → `orders[customer_id]` : **1-to-many**
- `orders[order_id]` → `order_items[order_id]` : **1-to-many**
- `products[product_id]` → `order_items[product_id]` : **1-to-many**
- `DateTable[Date]` → `orders[order_date]` : **1-to-many**

Single-direction filtering was used to keep the model simple and reduce unnecessary ambiguity.

---

## DAX & KPI Measures

Important measures used in the project include:

```DAX
Revenue =
SUM(order_items[line_sales])
```

```DAX
Total Orders =
DISTINCTCOUNT(orders[order_id])
```

```DAX
Delivered Orders =
CALCULATE(
    [Total Orders],
    orders[status] = "Delivered"
)
```

```DAX
Cancelled Orders =
CALCULATE(
    [Total Orders],
    orders[status] = "Cancelled"
)
```

```DAX
Cancellation Rate =
DIVIDE(
    [Cancelled Orders],
    [Total Orders]
)
```

```DAX
Quantity Sold =
SUM(order_items[quantity])
```

```DAX
Average Order Value =
DIVIDE(
    [Revenue],
    [Orders With Items]
)
```

Customer analysis also includes measures and calculated columns for:

- Active Customers
- Repeat Customers
- Repeat Customer Rate
- Revenue per Customer
- Orders per Customer
- Customer Lifetime Revenue
- Customer Segmentation
- Purchasing Frequency
- Retention Status

---

# Dashboard Pages

## 1. Executive E-Commerce Summary Dashboard

Provides a high-level overview of the business using:

- Total Revenue
- Total Orders
- Total Customers
- Quantity Sold
- Delivered Orders
- Average Order Value
- Active Customers
- Repeat Customer Rate
- Cancellation Rate
- Revenue Trend
- Payment Method Distribution
- Revenue by Category
- Top Products
- Revenue by City
- Key Business Insights

Example overall KPIs from the analysis include:

- Revenue: **₹79.79M** based on delivered item-level sales
- Total Orders: **500**
- Total Customers: **100**
- Delivered Orders: **349**
- Cancellation Rate: **10.6%**
- Active Customers: **96**
- Repeat Customer Rate: **88.54%** in the final executive dashboard

---

## 2. Sales Performance Dashboard

The Sales Performance Dashboard provides a focused view of overall sales performance and the main revenue drivers. It is designed to answer questions such as:

- How much revenue is being generated?
- How many orders are being placed?
- What is the average order value?
- How is revenue changing over time?
- Which categories, products and cities generate the most revenue?

### Key Analysis

- Revenue
- Total Orders
- Average Order Value
- Revenue Trend
- Revenue by Category
- Top 10 Products by Revenue
- Revenue by City
- Order Status Distribution

### Interactive Filters

- Date
- Category
- City
- Order Status

The dashboard uses KPI cards for headline metrics, a line chart for revenue trends, bar charts for category/product/city comparisons, and an order-status visual for operational context. Dynamic insight cards can also highlight the leading category, top product and leading city based on the selected filters.

---

## 3. Customer Analytics Dashboard

Focuses on customer acquisition, customer base and customer geography.

Key analysis:

- Total Customers
- Active Customers
- Repeat Customers
- Customer Revenue
- Customer Signup Trend
- Customer Segmentation
- Customers by City
- Revenue by City
- Repeat Customer Rate by City

---

## 4. Product & Category Dashboard

Focuses on product sales performance and inventory visibility.

Key analysis:

- Revenue by Category
- Quantity Sold
- Average Selling Price
- Total Stock
- Category Contribution
- Top 10 Products by Revenue
- Bottom 10 Products by Revenue
- Product Stock
- Inventory Health
- Products Requiring Business Attention

Business-attention logic can be used to flag cases such as:

- Out of Stock
- Replenishment Needed
- Potential Overstock
- No Sales

These are analytical flags for investigation and should be aligned with company-specific inventory thresholds in a real business setting.

---

## 5. Orders & Payment Dashboard

Focuses on order fulfillment and payment behavior.

Key analysis:

- Total Orders
- Delivered Orders
- Cancelled Orders
- Cancellation Rate
- Pending Orders
- Returned Orders
- Orders by Payment Method
- Revenue by Payment Method
- Order Trends
- Cancellation Trends
- Cancellation Rate by Payment Method

---

## 6. Customer Value & Retention Dashboard

The Customer Value & Retention analysis focuses on customer loyalty, purchasing frequency and revenue contribution. It includes:

- Repeat Customer Rate
- Revenue per Customer
- Orders per Customer
- Customer Segmentation
- High-Value Customers
- Customer Purchasing Frequency
- Revenue Contribution by Customer Segment
- Customer Recency and Retention Indicators

The objective is to identify high-value and frequent customers, understand retention behavior, and highlight customer groups that may require retention or growth attention.

---

## 7. Executive Summary Dashboard

The Executive Summary Dashboard consolidates the most important KPIs from the project for senior-management review:

- Total Revenue
- Total Orders
- Total Customers
- Average Order Value
- Repeat Customer Rate
- Cancellation Rate
- Total Quantity Sold
- Top Category

It also presents concise executive insights covering revenue drivers, customer value, payment behavior, cancellations, product attention and city-level opportunities, followed by recommendations supported by the dashboard analysis.

---

# Key Business Insights

### 1. Category Performance

Electronics is a leading revenue category and contributes a substantial share of delivered revenue. Electronics and Sports together represent approximately **63% of delivered revenue** in the current analysis.

### 2. Customer Value Concentration

Frequent and higher-value customers contribute a large share of revenue, highlighting the importance of retention and repeat purchasing.

### 3. Geographic Performance

Mumbai and Chennai are among the strongest revenue-generating cities in the current dataset. City-level analysis can be used to investigate differences in customer base, order volume and revenue.

### 4. Cancellation & Pending Orders

The overall cancellation rate is **10.6%**, while **71 orders are pending**. These are important operational metrics to monitor.

### 5. Payment Method Pattern

Net Banking has the highest order volume in the current dataset. Cash on Delivery shows a higher observed cancellation rate than the overall rate, making it an area for operational investigation rather than a direct causal conclusion.

### 6. Data Quality

The project identified **19 delivered orders without item-level details**. This should be resolved before using the dashboard as a fully reconciled financial reporting source.

---

# Business Recommendations

1. **Protect high-performing categories and products** by monitoring their sales velocity and availability.
2. **Strengthen customer retention** through targeted campaigns for frequent and high-value customers.
3. **Investigate cancellation patterns**, especially where payment methods or specific time periods show elevated cancellation rates.
4. **Improve inventory planning** by combining stock levels with actual sales velocity.
5. **Review city-level performance** to understand why some markets contribute more revenue than others.
6. **Improve data quality** by reconciling the missing item-level records for delivered orders.

> Recommendations are based on the patterns observed in the dashboard and should be validated with operational, financial and customer-level context before implementation.

---

# Tools & Technologies

- **Microsoft Power BI**
- **Power Query**
- **DAX**
- Data Modeling
- Data Visualization
- Business Intelligence

---

# Skills Demonstrated

- Data cleaning and transformation
- Relational data modeling
- Star-schema concepts
- DAX measures and calculated columns
- KPI development
- Interactive dashboard design
- Customer analytics
- Product and category analytics
- Operational analysis
- Business insight generation
- Data-quality validation

---

# Project Screenshots

The project contains the following Power BI report pages:

- Executive E-Commerce Summary
- Sales Performance
- Customer Analytics
- Product & Category
- Orders & Payment
- Customer Value & Retention

---

# Conclusion

This project demonstrates an end-to-end BI workflow in Power BI, from data preparation and modeling to DAX calculations, interactive reporting, business insights and recommendations.

The main objective was to move beyond displaying numbers and use the data to understand **revenue drivers, customer behavior, product performance and operational issues**.
