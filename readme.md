# Retail Supply Chain Analysis

A comprehensive SQL-based analysis of retail operations focusing on supply chain optimization, profitability insights, and operational efficiency metrics.

## 📊 Project Overview

This project analyzes retail order data to extract actionable insights for supply chain management, inventory optimization, and profit maximization. Using SQL queries on a retail dataset, we examine 15 critical business problems that impact supply chain decisions.

## 🗃️ Dataset

-   **Orders Table**: Complete retail order dataset with sales, profit, discount, and return information
-   **Calendar Table**: Time dimension for temporal analysis
-   **Database**: SQLite implementation for efficient querying

## 📋 Analysis Problems & Solutions

### 1. **Category Profitability Analysis**

**Problem**: Identify most profitable product categories for supply chain prioritization

```sql
SELECT 
    category,
    SUM(sales) AS total_sales,
    SUM(profit) AS total_profit
FROM orders
GROUP BY category
ORDER BY total_profit DESC;

```

**Key Insights**:

-   Technology leads with **17.4%** profit margin despite moderate sales
-   Furniture shows concerning **2.5%** margin requiring supplier renegotiation
-   Office Supplies maintains steady **17%** margin with reliable cash flow

----------

### 2. **Average Sales Per Unit by Category**

**Problem**: Understand unit economics for inventory allocation decisions

```sql
SELECT 
    Category,
    ROUND(SUM(Sales) / SUM(Quantity), 2) AS avg_sales_per_unit
FROM orders
GROUP BY Category
ORDER BY avg_sales_per_unit DESC;

```

**Key Insights**:

-   Technology commands premium pricing at **$120.51** per unit
-   Furniture mid-range at **$92.43** indicates bulk logistics optimization needs
-   Office Supplies at **$31.40** requires lean inventory and cost-efficient suppliers

----------

### 3. **Top Returned Products Analysis**

**Problem**: Identify quality control and supplier issues through return patterns

```sql
SELECT 
    "Product Name",
    COUNT(*) AS return_count
FROM orders
WHERE Returned = 'Yes'
GROUP BY "Product Name"
ORDER BY return_count DESC
LIMIT 5;

```

**Key Insights**:

-   Staple envelopes and KI tables lead returns (4 each)
-   Points to potential quality or shipping damage issues
-   Requires supplier audits and enhanced packaging standards

----------

### 4. **Profit Margin by Product Category**

**Problem**: Evaluate category-level profitability for resource allocation

```sql
SELECT 
    Category,
    ROUND(SUM(Profit), 2) AS total_profit,
    ROUND(SUM(Sales), 2) AS total_sales,
    ROUND(SUM(Profit) * 100.0 / SUM(Sales), 2) AS profit_margin_pct
FROM orders
GROUP BY Category
ORDER BY profit_margin_pct DESC;

```

**Key Insights**:

-   Technology & Office Supplies both achieve **17%+** margins
-   Furniture's **2.5%** margin likely unprofitable after logistics costs
-   Suggests need for warehouse space reallocation to high-margin categories

----------

### 5. **Regional and Segment Performance**

**Problem**: Optimize supply chain resources based on geographic and customer segment profitability

```sql
SELECT 
    Region,
    Segment,
    ROUND(SUM(Sales), 2) AS total_sales,
    ROUND(SUM(Profit), 2) AS total_profit
FROM orders
GROUP BY Region, Segment
ORDER BY total_profit DESC;

```

**Key Insights**:

-   West Region Consumers generate highest profit (**$57.4K**)
-   Central Consumer segment underperforms despite high sales
-   Indicates regional logistics efficiency variations requiring optimization

----------

### 6. **Sales Velocity by Category**

**Problem**: Determine inventory replenishment frequency and warehouse planning

```sql
SELECT 
    Category,
    ROUND(SUM(Quantity) * 1.0 / COUNT(DISTINCT "Order Date"), 2) AS sales_velocity
FROM orders
WHERE "Order Date" IS NOT NULL
GROUP BY Category
ORDER BY sales_velocity DESC;

```

**Key Insights**:

-   Office Supplies fastest moving (**19.95 units/day**) requiring frequent restocking
-   Technology/Furniture slower turnover but higher value needing optimized storage
-   Dictates different inventory strategies: lean vs. bulk approaches

----------

### 7. **Profit Per Unit Analysis**

**Problem**: Assess unit-level profitability for inventory investment decisions

```sql
SELECT 
    Category,
    ROUND(SUM(Profit) / SUM(Quantity), 2) AS profit_per_unit
FROM orders
WHERE Quantity > 0
GROUP BY Category
ORDER BY profit_per_unit DESC;

```

**Key Insights**:

-   Technology delivers **$20.96 profit per unit** (4x higher than Office Supplies)
-   Furniture's **$2.30** per unit highlights cost structure issues
-   Guides inventory capital allocation prioritization

----------

### 8. **Return Rate by Category**

**Problem**: Identify supply chain risk factors and quality issues

```sql
SELECT 
    Category,
    COUNT(CASE WHEN Returned = 'Yes' THEN 1 END) * 1.0 / COUNT(*) AS return_rate
FROM orders
GROUP BY Category
ORDER BY return_rate DESC;

```

**Key Insights**:

-   Technology has highest return rate (**8.45%**) despite profitability
-   Suggests quality control gaps in high-value products
-   Requires enhanced pre-shipment testing and supplier quality audits

----------

### 9. **Gross Margin Return on Investment (GMROI)**

**Problem**: Evaluate inventory investment efficiency across categories

```sql
SELECT 
    Category,
    ROUND(SUM(Profit) / NULLIF(SUM(Sales - Profit), 0), 2) AS gmroi
FROM orders
GROUP BY Category
ORDER BY gmroi DESC;

```

**Key Insights**:

-   Technology and Office Supplies both achieve **0.21** GMROI
-   Furniture significantly lower at **0.03** indicating poor inventory ROI
-   Guides capital allocation and inventory investment decisions

----------

### 10. **Average Discount by Category**

**Problem**: Analyze pricing strategy effectiveness and margin erosion

```sql
SELECT 
    Category,
    ROUND(AVG(Discount), 2) AS avg_discount
FROM orders
GROUP BY Category
ORDER BY avg_discount DESC;

```

**Key Insights**:

-   Furniture requires highest discounts (**17%**) suggesting overstock issues
-   Technology maintains pricing power with lower discounts (**13%**)
-   Office Supplies mid-level discounts (**16%**) reflect competitive market

----------

### 11. **Return Rate by Region**

**Problem**: Identify regional logistics and quality issues

```sql
SELECT 
    Region,
    COUNT(CASE WHEN Returned = 'Yes' THEN 1 END) * 1.0 / COUNT(*) AS return_rate
FROM orders
GROUP BY Region
ORDER BY return_rate DESC;

```

**Key Insights**:

-   West Region alarmingly high returns (**15.3%**) despite profitability
-   Points to logistics/shipping issues or customer expectation mismatches
-   Central Region's low rate (**4%**) offers benchmarking opportunity

----------

### 12. **Discount Effectiveness Analysis**

**Problem**: Measure profitability impact of discount strategies

```sql
SELECT 
    Category,
    ROUND(SUM(Profit) / NULLIF(SUM(Discount), 0), 2) AS profit_per_discount_dollar
FROM orders
WHERE Discount > 0
GROUP BY Category
ORDER BY profit_per_discount_dollar DESC;

```

**Key Insights**:

-   Technology discounts generate **$53.63 profit per discount dollar**
-   Furniture discounts destroy value (**-$107.57**) indicating pricing issues
-   Suggests need for category-specific discount strategies

----------

### 13. **Regional Profit Margins**

**Problem**: Evaluate regional operational efficiency and cost structures

```sql
SELECT 
    Region,
    ROUND(SUM(Profit) * 100.0 / SUM(Sales), 2) AS profit_margin_pct
FROM orders
GROUP BY Region
ORDER BY profit_margin_pct DESC;

```

**Key Insights**:

-   West Region leads with **14.9%** margin (efficient coastal logistics)
-   Central Region lags at **7.9%** indicating transportation/warehousing inefficiencies
-   Nearly 2x margin gap requires operational deep-dive and improvement initiatives

----------

### 14. **Top Returning Cities Analysis**

**Problem**: Identify geographic hotspots for logistics and quality issues

```sql
SELECT 
    City,
    COUNT(*) AS total_returns
FROM orders
WHERE Returned = 'Yes'
GROUP BY City
ORDER BY total_returns DESC
LIMIT 10;

```

**Key Insights**:

-   Los Angeles dominates with **117 returns** (delivery/quality challenges)
-   West Coast concentration suggests regional systemic issues
-   Smaller cities in top 10 indicate broader logistics problems

----------

### 15. **Sub-Category Profit Contribution**

**Problem**: Identify profit concentration and optimization opportunities

```sql
SELECT 
    "Sub-Category",
    ROUND(SUM(Profit) * 100.0 / (SELECT SUM(Profit) FROM orders), 2) AS profit_contribution_pct
FROM orders
GROUP BY "Sub-Category"
ORDER BY profit_contribution_pct DESC;

```

**Key Insights**:

-   Top 3 sub-categories (**Copiers, Phones, Accessories**) generate **49.6%** of total profit
-   Tables show **-6.2%** contribution requiring immediate cost restructuring
-   Extreme profit concentration suggests need for portfolio optimization

----------

## 🎯 Strategic Recommendations

### **High Priority Actions**

1.  **Technology Supply Chain Resilience**
    
    -   Secure long-term supplier contracts for high-margin tech products
    -   Implement enhanced quality control to reduce 8.45% return rate
    -   Maintain premium pricing power through strategic sourcing
2.  **Furniture Category Restructuring**
    
    -   Conduct comprehensive supplier cost renegotiation
    -   Evaluate product line discontinuation for unprofitable items
    -   Optimize warehousing and logistics for bulky items
3.  **Regional Optimization**
    
    -   Deep-dive Central Region's cost structure (7.9% vs 14.9% margin gap)
    -   Address West Coast return issues (15.3% rate)
    -   Replicate West Region best practices across other territories
4.  **Inventory Strategy Alignment**
    
    -   Fast-frequency restocking for Office Supplies (19.95 units/day velocity)
    -   Premium storage optimization for high-value Technology products
    -   Reduce furniture inventory investment given poor GMROI

### **Success Metrics to Track**

-   Category profit margins (Target: >15% for all categories)
-   Regional return rates (Target: <5% across all regions)
-   Inventory turnover by category
-   GMROI improvement for underperforming categories
-   Supply chain cost as % of sales by region

## 🛠️ Technical Implementation

-   **Database**: SQLite
-   **Tools**: Python, Pandas, SQL
-   **Analysis Method**: Descriptive analytics with business intelligence focus
-   **Visualization**: Tabular analysis with strategic insights

## 📈 Business Impact

This analysis provides data-driven insights for:

-   Supply chain optimization and cost reduction
-   Inventory investment allocation decisions
-   Supplier relationship prioritization
-   Regional operational improvements
-   Product portfolio optimization strategies

----------

_This analysis transforms raw retail data into actionable supply chain intelligence, enabling data-driven decision making for operational excellence and profit optimization._
