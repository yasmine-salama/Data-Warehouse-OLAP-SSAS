# Data Warehouse – OLAP Cube Analysis and Customization using SSAS

![Data Warehouse](https://img.shields.io/badge/Domain-Data%20Warehouse-blue)
![SQL Server](https://img.shields.io/badge/SQL%20Server-SSAS-red)
![OLAP](https://img.shields.io/badge/Technology-OLAP-orange)

## Project Overview

This academic mini-project was carried out as part of the **Data Warehouse** module during the second year of the DUT in Data Engineering at the École Supérieure de Technologie d'El Kelâa des Sraghna (ESTK), Cadi Ayyad University.

The project focuses on the analysis and customization of **OLAP (Online Analytical Processing) cubes** using Microsoft SQL Server Analysis Services (SSAS).

Based on the **Adventure Works DW2022** database, the project covers the construction and exploration of a multidimensional analytical environment, the creation of customized analytical views, the development of calculated measures using MDX, and the use of SQL queries to generate business reports and study aggregation strategies.

## Objectives

- Understand the architecture and operation of a Data Warehouse.
- Create, deploy and explore an OLAP cube using SSAS.
- Analyze sales data across multiple dimensions.
- Study relationships between customers, promotions and sales performance.
- Perform temporal analysis to identify trends and seasonal variations.
- Build customized geographical and product-based analytical views.
- Create calculated measures using MDX.
- Explore aggregation techniques, including `CUBE` and `ROLLUP`.
- Write SQL OLAP queries and compare relational SQL with multidimensional analysis.

## Technologies and Tools

| Technology | Purpose |
|---|---|
| Microsoft SQL Server | Database management |
| SQL Server Analysis Services (SSAS) | OLAP cube creation and multidimensional analysis |
| Visual Studio 2022 | Project development and cube modeling |
| SQL Server Management Studio (SSMS 21) | Database administration and SQL/MDX execution |
| SQL | Relational queries and analytical reports |
| MDX | Multidimensional calculations and OLAP analysis |
| Adventure Works DW2022 | Source Data Warehouse database |

---

# Project Implementation

## 1. Initial Multidimensional Model and Sales Analysis

### 1.1. Data Source Configuration

The first step consisted of creating an Analysis Services project in Visual Studio 2022 and configuring a Data Source connected to the `Adventure Works DW2022` database.

Main operations:
- Create an Analysis Services project.
- Add a Data Source.
- Configure the SQL Server connection.
- Select the `Adventure Works DW2022` database.
- Test and validate the connection.

### 1.2. Data Source View Creation

A Data Source View (DSV) was created to provide a logical representation of the tables required by the cube.

The initial model included the central fact table:
- `FactInternetSales`

And the main dimensions:
- `DimCustomer`
- `DimDate`
- `DimProduct`
- `DimPromotion`

The DSV makes it possible to visualize the tables and their relationships with the fact table.

### 1.3. OLAP Cube Creation, Deployment and Processing

An OLAP cube was created from the Data Source View, using `FactInternetSales` as its central fact table.

After configuring the cube structure, it was deployed to the Analysis Services server and processed so that its data became available for browsing and analysis.

The Browse interface was then used to explore measures and dimensions from different analytical perspectives.

### 1.4. Task 1 – Cross-Analysis of Sales by Customer and Promotion

**Objective:** Analyze sales by customer and investigate the contribution of promotional campaigns.

**Customer analysis:** The `DimCustomer` dimension was explored through the cube's Browse interface. Customer attributes, including customer keys and first names, were used to display sales amounts by customer.

**Promotion analysis:** The `DimPromotion` dimension was integrated into the analysis. Attributes included:
- `English Promotion Name`
- `English Promotion Type`
- `Promotion Key`

Sales amounts were examined according to customers and promotions to compare promotional offers and their distribution across customers.

**Interpretation and business considerations:**
- A significant share of sales may come from purchases without discounts.
- Volume-based promotions can contribute to sales activity.
- Clearance promotions may increase sales volume while putting pressure on profitability.

**Recommendations:**
- Prefer targeted promotions over generalized discounts.
- Evaluate promotions using profitability indicators, not only sales volume.
- Analyze customer purchasing behavior before launching campaigns.
- Monitor the effect of discounts on profit margins.

## 2. Advanced Temporal Analysis

### 2.1. Integration of the Date Dimension

The `DimDate` dimension was used to support temporal analysis. The analysis was organized around:
- Calendar Year
- Calendar Quarter
- Month

The `English Month Name` attribute was used to display monthly sales. The month ordering was configured using `MonthNumberOfYear` as the key/order attribute so that months appear chronologically rather than alphabetically.

### 2.2. Calculated Measure Using MDX

A calculated measure named `Variation Ventes` was created to calculate the difference between sales in the current period and sales in the previous period.

```mdx
[Measures].[Sales Amount] -
(
    [Measures].[Sales Amount],
    [Dim Date].[Hierarchie].CurrentMember.PrevMember
)
```

This expression compares the current member's sales amount with the amount associated with the previous member in the temporal hierarchy.

### 2.3. Temporal Exploration and Interpretation

The cube was explored at year, quarter and month levels. The calculated measure was used to observe changes between periods and identify increases or decreases in sales.

**Business recommendations:**
- Anticipate seasonal demand fluctuations.
- Adapt inventory levels to expected sales.
- Schedule marketing campaigns during high-activity periods.
- Investigate periods of declining sales.
- Use historical trends to support forecasting.

## 3. Creation of Complex Analytical Views

This part extended the initial model to support geographical and product-based analysis. Additional dimensions included:
- `DimGeography`
- `DimProductCategory`
- `DimProductSubcategory`

### 3.1. View 1 – Sales Analysis by Region

**Objective:** Analyze sales according to geographical regions and product categories.

**Geographical attributes:** `DimGeography` provided attributes such as:
- City
- State Province
- Country Region
- Geography Key

**Dimension relationship:** Since geographical information is linked to customers through `GeographyKey`, a referenced relationship was configured between `DimCustomer` and `DimGeography`, using `GeographyKey`.

This relationship makes geographical attributes available for analyzing measures originating from `FactInternetSales`.

**Product hierarchy:** The product analysis was extended through:
- `DimProduct`
- `DimProductSubcategory`
- `DimProductCategory`

The relationship between `DimProduct` and `DimProductSubcategory` uses `ProductSubcategoryKey`. The product hierarchy supports analysis at product, subcategory and category levels.

**Interpretation and recommendations:**
- Identify geographical areas generating higher sales.
- Adapt product availability to regional demand.
- Improve inventory distribution.
- Develop marketing campaigns based on regional purchasing patterns.

### 3.2. View 2 – Sales Analysis by Product Type

**Objective:** Analyze sales according to product types and customer profiles.

The cube was explored by combining product attributes with customer information, including:
- Product categories and subcategories
- `English Product Name`
- `Customer First Name`
- `English Occupation`

Sales amounts were examined at product level, together with measures such as `Total Product Cost` and `Order Quantity`.

**Calculated measure – Average Cost per Product**

A calculated measure named `Coût Moyen par Produit` was created:

```mdx
[Measures].[Total Product Cost] /
[Measures].[Order Quantity]
```

It calculates the average product cost using total product cost and ordered quantity.

**Calculated measure – Average Cost per Sale**

A second calculated measure named `Coût Moyen par Vente` was created:

```mdx
[Measures].[Total Product Cost] /
[Measures].[Fact Internet Sales Count]
```

It estimates the average product cost per sales transaction using total product cost and the sales count measure.

**Interpretation and recommendations:**
- Identify products with high average costs.
- Compare product costs across customer segments.
- Improve product portfolio management.
- Monitor cost indicators alongside sales revenue.
- Use customer profiles to support product recommendations and marketing decisions.

## 4. Aggregation Optimization and Performance Comparison

This part examined analytical aggregation strategies and the use of SQL and MDX for business analysis.

### 4.1. Advanced SQL Aggregations: ROLLUP and CUBE

**ROLLUP** generates hierarchical subtotals and a grand total. It is useful for geographical reports, for example sales by country and state.

Illustrative example:

```sql
SELECT
    g.EnglishCountryRegionName AS Country,
    g.StateProvinceName AS State,
    SUM(f.SalesAmount) AS TotalSales
FROM FactInternetSales f
JOIN DimCustomer c
    ON f.CustomerKey = c.CustomerKey
JOIN DimGeography g
    ON c.GeographyKey = g.GeographyKey
GROUP BY ROLLUP (
    g.EnglishCountryRegionName,
    g.StateProvinceName
);
```

**CUBE** generates aggregations for all possible combinations of the selected grouping dimensions. It is useful for comparing sales across geographical regions and product categories.

Illustrative example:

```sql
SELECT
    g.EnglishCountryRegionName AS Country,
    pc.EnglishProductCategoryName AS Category,
    SUM(f.SalesAmount) AS TotalSales
FROM FactInternetSales f
JOIN DimCustomer c
    ON f.CustomerKey = c.CustomerKey
JOIN DimGeography g
    ON c.GeographyKey = g.GeographyKey
JOIN DimProduct p
    ON f.ProductKey = p.ProductKey
JOIN DimProductSubcategory ps
    ON p.ProductSubcategoryKey = ps.ProductSubcategoryKey
JOIN DimProductCategory pc
    ON ps.ProductCategoryKey = pc.ProductCategoryKey
GROUP BY CUBE (
    g.EnglishCountryRegionName,
    pc.EnglishProductCategoryName
);
```

### 4.2. SQL OLAP Queries

The project requirements include three analytical SQL query themes.

**Query 1 – Top 10 Promotions by Sales**

Objective: Identify the promotions generating the highest sales amounts.

```sql
SELECT TOP 10
    p.EnglishPromotionName,
    SUM(f.SalesAmount) AS TotalSales
FROM FactInternetSales f
JOIN DimPromotion p
    ON f.PromotionKey = p.PromotionKey
GROUP BY p.EnglishPromotionName
ORDER BY TotalSales DESC;
```

**Query 2 – Sales Distribution by Region with Subtotals**

Objective: Calculate sales by geographical region, including subtotals and a grand total.

```sql
SELECT
    g.EnglishCountryRegionName AS Country,
    g.StateProvinceName AS State,
    SUM(f.SalesAmount) AS TotalSales
FROM FactInternetSales f
JOIN DimCustomer c
    ON f.CustomerKey = c.CustomerKey
JOIN DimGeography g
    ON c.GeographyKey = g.GeographyKey
GROUP BY ROLLUP (
    g.EnglishCountryRegionName,
    g.StateProvinceName
)
ORDER BY Country, State;
```

**Query 3 – Monthly Sales Evolution**

Objective: Analyze monthly sales over a selected period.

```sql
SELECT
    d.CalendarYear,
    d.CalendarQuarter,
    d.MonthNumberOfYear,
    d.EnglishMonthName,
    SUM(f.SalesAmount) AS MonthlySales
FROM FactInternetSales f
JOIN DimDate d
    ON f.OrderDateKey = d.DateKey
WHERE d.CalendarYear BETWEEN 2012 AND 2013
GROUP BY
    d.CalendarYear,
    d.CalendarQuarter,
    d.MonthNumberOfYear,
    d.EnglishMonthName
ORDER BY
    d.CalendarYear,
    d.MonthNumberOfYear;
```

The date range can be adjusted to match the analysis period required.

### 4.3. MDX Analysis – Top 10 Promotions

The top-promotions analysis was also expressed using MDX. The `TOPCOUNT` function selects the ten promotion members with the highest sales amounts.

```mdx
SELECT
{
    [Measures].[Sales Amount]
} ON COLUMNS,

TOPCOUNT(
    [Dim Promotion].[English Promotion Name]
        .[English Promotion Name].MEMBERS,
    10,
    [Measures].[Sales Amount]
) ON ROWS

FROM [tache-1]
```

This query uses the multidimensional structure of the cube and its measures.

### 4.4. SQL and OLAP Performance Comparison

The comparison focused on:
- Query execution time.
- Aggregation strategies.
- Data organization.
- Subtotal calculation.
- Reuse of pre-calculated analytical measures.

Relational SQL queries may perform joins, grouping, sorting and aggregation during execution. An OLAP cube can use its multidimensional structure and configured aggregations to support repeated analytical queries.

**Optimization recommendations:**
- Identify frequently requested reports.
- Configure aggregations according to analytical needs.
- Optimize dimension relationships and hierarchies.
- Avoid unnecessary joins and calculations.
- Use pre-calculated measures when appropriate.
- Compare execution times under equivalent conditions.

---

## Conclusion

This mini-project provided practical experience in designing, developing and exploring a multidimensional Data Warehouse using Microsoft SQL Server Analysis Services.

The work covered customer and promotion analysis, temporal analysis, complex analytical views, calculated MDX measures, SQL OLAP queries and aggregation optimization.

It demonstrated how OLAP cubes can transform transactional data into structured analytical information that supports Business Intelligence and decision-making.

## Skills Acquired

- Data Warehouse Architecture
- Multidimensional Data Modeling
- OLAP Cube Design and Deployment
- SQL Server Analysis Services (SSAS)
- SQL and MDX
- Business Intelligence
- Data Analysis
- Temporal and Geographical Analysis
- Calculated Measures
- SQL Aggregations: CUBE and ROLLUP
- Query and Aggregation Optimization

## Academic Context

- **Institution:** École Supérieure de Technologie d'El Kelâa des Sraghna (ESTK), Cadi Ayyad University
- **Program:** Second-Year DUT – Data Engineering
- **Module:** Data Warehouse
- **Academic Year:** 2025–2026
- **Project:** Analysis and Customization of OLAP Cubes using SSAS
