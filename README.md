# E-commerce-Database-Setup-Sales-Analysis-SQL-
##  Introduction
Welcome to my SQL portfolio project! As a beginner learning Data Analytics, I built this project to demonstrate how an online store can turn a basic spreadsheet of daily sales
transactions into clear, actionable business insights using **SQL** and **MySQL Workbench**.

---

##  10 Business Problems & SQL Solutions
Hiring managers often say a business cannot grow just by looking at a long spreadsheet. To prove my analytical mindset, I wrote custom SQL queries inside **MySQL Workbench** to solve these **10 specific, real-world business problems**:

### 1. High-Level Executive Metrics (KPIs)
*   **The Problem:** The executive team needs a high-level pulse check on overall store health.
*   **My Insights:** I calculated a total volume of **1,200 orders**, lifetime sales revenue of **$1,264,761.96**, and an Average Order Value (AOV) of **$1,053.97**.
*   **The Code:**
```sql
SELECT 
    COUNT(OrderID) AS Total_Orders,
    SUM(TotalPrice) AS Total_Revenue,
    AVG(TotalPrice) AS Average_Order_Value
FROM Orders;
```

### 2. Product Catalog Pricing Strategy
*   **The Problem:** Product management needs to verify inventory items and identify our maximum retail price ceilings.
*   **My Insights:** I discovered that Laptops and Tablets represent our highest price tier, topping out at **$699.93** and **$699.88**.
*   **The Code:**
```sql
SELECT Product, MAX(UnitPrice)
FROM Orders
GROUP BY Product
ORDER BY MAX(UnitPrice) DESC;
```

### 3. High-Value Revenue Leakage Audit
*   **The Problem:** Finance needs to track high-risk patterns where expensive sales were completely lost.
*   **My Insights:** I isolated all transactions **over $1,000** that ended up with a status of *Cancelled* or *Returned* to analyze financial vulnerability.
*   **The Code:**
```sql
SELECT * FROM orders
WHERE (OrderStatus = 'Cancelled' OR OrderStatus = 'Returned') 
  AND TotalPrice > 1000
ORDER BY TotalPrice DESC;
```

### 4. Organic vs. Promotion Sales Ratio
*   **The Problem:** Marketing wants to see how well the store sells products organically without relying on discounts.
*   **My Insights:** I filtered out promotional noise to separate pure organic demand from coupon-driven traffic.
*   **The Code:**
```sql
SELECT * FROM orders 
WHERE CouponCode IS NULL OR CouponCode = '';
```

### 5. Maximum Marketing Channel Yield
*   **The Problem:** The advertising team wants to know exactly which referral platform brings in the absolute highest total financial return.
*   **My Insights:** I calculated total revenue by referral stream and discovered that **Instagram** is our number one marketing source, generating **$275,285.45**.
*   **The Code:**
```sql
SELECT ReferralSource, SUM(TotalPrice) AS TotalRevenue
FROM Orders
GROUP BY ReferralSource
ORDER BY TotalRevenue DESC
LIMIT 1;
```

### 6. Checkout Friction & Abandonment Behavior
*   **The Problem:** The user-experience (UX) team wants to see if customers drop out of the checkout lane due to friction.
*   **My Insights:** I found that orders that ended up being *Cancelled* left an average of **6 items** abandoned behind in their digital shopping carts.
*   **The Code:**
```sql
SELECT ROUND(AVG(ItemsINCart), 0) AS CancelledItemLeft 
FROM Orders
WHERE OrderStatus = 'Cancelled';
```

### 7. Customer Payment Preferences
*   **The Problem:** The operations team needs to optimize payment gateway partnerships based on what shoppers actually use.
*   **My Insights:** I ranked payment popularity, proving **Online payments** are the most common (258 orders), closely followed by Cash (246 orders).
*   **The Code:**
```sql
SELECT PaymentMethod, COUNT(OrderID) AS Total_orders
FROM Orders
GROUP BY PaymentMethod
ORDER BY Total_orders DESC;
```

### 8. Multi-Year Sales Growth Trends
*   **The Problem:** Executives want to see the macro-financial trajectory of the store across multiple calendar years.
*   **My Insights:** I extracted years from our dates to track annual revenue performance across 2023 (**$552,643.24**), 2024 (**$480,235.87**), and 2025 (**$231,882.85**).
*   **The Code:**
```sql
SELECT YEAR(OrderDate) AS Sales_Year, SUM(TotalPrice) AS Total_Revenue
FROM orders
GROUP BY YEAR(OrderDate)
ORDER BY Sales_Year;
```

### 9. High-Value Repeat Buyers (Customer Lifetime Value)
*   **The Problem:** It costs more to find new shoppers than to keep old ones. Marketing needs a list of VIP repeat customers to reward.
*   **My Insights:** I filtered for loyal customers who placed **2 or more separate orders**, highlighting our highest-lifetime-value shopper (`C38840`) who spent **$5,723.23**.
*   **The Code:**
```sql
SELECT CustomerID, COUNT(OrderID) AS TOTAL_ORDERS, SUM(TotalPrice) AS Total_Spent
FROM orders
GROUP BY CustomerID
HAVING COUNT(OrderID) >= 2
ORDER BY Total_Spent DESC;
```

### 10. Data Integrity & Financial Math Audit
*   **The Problem:** Before trusting any data insights, an analyst must verify that the database calculations are completely accurate.
*   **My Insights:** I conducted a data integrity check by scanning for any system errors where `TotalPrice` did not perfectly equal `Quantity * UnitPrice`.
*   **The Code:**
```sql
SELECT * FROM orders
WHERE TotalPrice != (Quantity * UnitPrice);
```

---

##  How I Built the Data Pipeline & Cleaned the Schema
Before running the queries above, I manually set up the database environment and handled essential data cleaning:
1.  **Ingestion:** Set up a local schema named `ecommerce_sales` and utilized the MySQL Workbench Import Wizard to stream a raw transactions `.csv` file into an `orders` table.
2.  **Data Structural Fix:** I noticed the column header for marketing channels was misspelled as `RefferralSource` (with a double 'f'). I wrote an **`ALTER TABLE`** command to reshape the layout column name to `ReferralSource`. Correcting this structural bug ensured all my analytical queries executed flawlessly.

```sql
ALTER TABLE Orders 
RENAME COLUMN RefferralSource TO ReferralSource;
```

---

##  Lessons Learned
Building this project taught me several critical lessons about working as a data analyst:

*   **Business Intelligence over Syntax:** I learned that writing SQL commands is easy, but understanding *why* you are writing them to solve a corporate financial bottleneck is what truly adds value to an organization.
*   **The Reality of Dirty Data:** Raw spreadsheets almost always contain column typos, spelling mistakes, or uneven styling. Catching the `RefferralSource` column mismatch early taught me to verify database architecture layouts before running deep analysis.
*   **Data Integrity Matters:** Creating data audits (like checking if `TotalPrice = Quantity * UnitPrice`) is a crucial first step. If your baseline database mathematics are broken, your executive summaries will be incorrect.
*
