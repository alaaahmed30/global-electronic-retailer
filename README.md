# Global Electronics Retailer — Power BI Dashboard

## 📊 Project Overview

An interactive Power BI dashboard analyzing the sales performance, profitability, customer distribution, brand performance, and delivery operations of a global electronics retailer.

The project transforms raw transactional, customer, and product data into an interactive business intelligence report that allows users to explore performance across different years, categories, brands, countries, and customer segments.

> **Note:** The business questions and analysis objectives in this project were self-defined based on the available dataset.

---

## 🎯 Business Questions

The dashboard was designed to answer the following questions:

### 1. Product Category Performance
- How do sales, costs, and profit margins differ across product categories?
- Which categories contribute the most to overall sales and profitability?

### 2. Customer Geographic Distribution
- How are customers distributed across continents, countries, and cities?
- How does the number of active customers compare with the total customer base?

### 3. Sales Performance
- How have total sales changed over time?
- What is the year-to-date sales performance?

### 4. Brand Performance
- Which brands generate the highest sales?
- How do the leading brands compare in terms of sales and profit?

### 5. Delivery Performance
- What is the average delivery time?
- How does average delivery time change over time?

### 6. Sales Channel Performance
- How does Average Order Value (AOV) differ between online and in-store sales?

---

## 📌 Dashboard Pages

### Page 1 — Sales Performance

The first page focuses on overall sales and profitability.

**Key KPIs:**
- Total Sales
- Total Profit
- Total Cost
- Total Orders
- Average Order Value (AOV)
- Profit Margin

**Visualizations:**
- Total Sales by Year
- Total Orders by Country
- Sales, Cost, and Profit Margin by Product Category
- Total Sales and Total Profit by Brand

**Filters:**
- Year
- Product Category
- Brand

---

### Page 2 — Customers & Operations

The second page focuses on customer distribution and operational performance.

**Key KPIs:**
- Total Sales
- Total Profit
- Total Cost
- Total Orders
- Total Customers
- Profit Margin
- Sales YTD

**Visualizations:**
- Customer distribution by geographic hierarchy
- Average Delivery Days by Month
- Total Profit by Country
- Active Customers vs Total Customers

**Filters:**
- Country
- Product Category
- Brand

The geographic analysis uses the following drill-down hierarchy:

**Continent → Country → City**

---

## 🧹 Data Cleaning & Preparation

Data preparation was performed using **Power Query**.

The main preparation steps included:

- Reviewing data types across the different tables.
- Investigating data-quality errors in customer ZIP/postal code data.
- Reviewing missing values in the `Delivery Date` field.
- Investigating the business meaning behind missing delivery dates.
- Validating relationships between fact and dimension tables.
- Preparing the data for time-based analysis.

### Delivery Date Handling

A large number of records contained blank delivery dates. After investigating the data, these records were identified as **in-store purchases**, where a delivery does not take place.

Therefore, blank delivery dates were not treated as automatically invalid data.

For delivery-time analysis, only transactions with an actual delivery date were included.

---

## 🏗️ Data Model

The report uses a dimensional data model consisting of:

- `fact_sales`
- `dim_products`
- `dim_customers`
- `DateTable`

The `DateTable` is connected to the sales fact table through the order date and is used for time-based analysis such as:

- Year
- Month
- Quarter
- Year-Month
- Year-to-date calculations

The model uses relationships between the sales fact table and customer/product dimensions to allow filters and calculations to flow through the report.

---

## 🧮 Key DAX Measures

### Total Sales

```DAX
Total Sales =
SUMX(
    fact_sales,
    fact_sales[Quantity] *
        RELATED(dim_products[Unit Price USD])
)
```

### Total Cost

```DAX
Total Cost =
SUMX(
    fact_sales,
    fact_sales[Quantity] *
        RELATED(dim_products[Unit Cost USD])
)
```

### Total Profit

```DAX
Total Profit =
[Total Sales] - [Total Cost]
```

### Profit Margin

```DAX
Profit Margin =
DIVIDE(
    [Total Profit],
    [Total Sales]
)
```

### Total Customers

```DAX
Total Customers =
DISTINCTCOUNT(dim_customers[CustomerKey])
```

### Active Customers

```DAX
Active Customers =
DISTINCTCOUNT(fact_sales[CustomerKey])
```

### Sales YTD

```DAX
Sales YTD =
TOTALYTD(
    [Total Sales],
    DateTable[Date]
)
```

### Average Delivery Days

```DAX
Average Delivery Days =
AVERAGEX(
    FILTER(
        fact_sales,
        NOT ISBLANK(fact_sales[Delivery Date])
    ),
    DATEDIFF(
        fact_sales[Order Date],
        fact_sales[Delivery Date],
        DAY
    )
)
```

### Average Order Value

```DAX
Average Order Value =
DIVIDE(
    [Total Sales],
    DISTINCTCOUNT(fact_sales[Sales Order Number])
)
```

---

## 🔍 Analysis Areas

The dashboard allows users to investigate:

- Sales and profitability by product category
- Sales and profit contribution by brand
- Customer distribution across geographic levels
- Active versus total customers
- Sales trends over time
- Year-to-date sales performance
- Average delivery time
- Profit contribution by country
- Average order value across sales channels

All measures dynamically respond to the available filters and slicers.

---

## 🛠️ Tools & Technologies

- **Microsoft Power BI**
- **Power Query** — data cleaning and transformation
- **DAX** — business calculations and measures
- **Data Modeling** — relationships and dimensional modeling

---

## 📷 Dashboard Preview

### Sales Performance

![Sales Performance Dashboard](images/sales-performance.png)

### Customers & Operations

![Customers and Operations Dashboard](images/customer-operations.png)

---

## 📁 Project Structure

```text
Global-Electronics-Retailer/
│
├── README.md
├── Global Electronics Retailer.pbix
│
└── images/
    ├── sales-performance.png
    └── customer-operations.png
```

---

## 📚 Dataset

**Global Electronics Retailer — Maven Analytics Data Playground**

The dataset was used to analyze the retailer's sales, customers, products, brands, and delivery performance.

**Source:** Maven Analytics Data Playground

