# 📊 AtliQ Hardware - Sales Insights Dashboard

![Tableau](https://img.shields.io/badge/Tableau-E97627?style=for-the-badge&logo=Tableau&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-005C84?style=for-the-badge&logo=mysql&logoColor=white)

> **A Business Intelligence solution providing real-time sales insights for AtliQ Hardware across multiple Indian markets.**

![Dashboard Preview](Dashboard.png)

---

## 🎯 Problem Statement

AtliQ Hardware, a computer hardware supplier operating across India, was facing challenges in tracking sales performance across different markets. The sales director needed a way to:

- Monitor revenue trends across **15+ markets** in real-time
- Identify top-performing customers and products
- Analyze sales patterns over time (2017-2020)
- Filter data dynamically by year, month, and market

Manual Excel reports were time-consuming and lacked interactivity. This project delivers an **automated, interactive Tableau dashboard** powered by a MySQL database.

---

## 🏗️ Architecture

```mermaid
flowchart LR
    A[MySQL Database<br/>Star Schema] --> B[Tableau Desktop]
    B --> C[Interactive Dashboard]
    
    style A fill:#336791,color:#fff
    style B fill:#E97627,color:#fff
    style C fill:#4CAF50,color:#fff
```

**Tech Stack:**
- **Database:** MySQL 8.0
- **Visualization:** Tableau Desktop 2024.3
- **Data Model:** Star Schema (1 Fact + 4 Dimension Tables)

---

## 📁 Data Model

The database follows a **Star Schema** design optimized for analytics:

### Fact Table: `transactions`
- **Purpose:** Records every sales transaction
- **Key Metrics:**
  - `sales_amount` (Revenue in INR/USD)
  - `sales_qty` (Quantity sold)
  - `profit_margin`, `cost_price`
- **Foreign Keys:** Links to customers, products, markets, dates

### Dimension Tables:
| Table | Description | Key Attributes |
|-------|-------------|----------------|
| `customers` | Customer master data | Customer Name, Type (Brick & Mortar / E-Commerce) |
| `products` | Product catalog | Product Type (Own Brand / Distribution) |
| `markets` | Geographic zones | Market Name, Zone (North/South/Central) |
| `date` | Date dimension | Year, Month, Date |

---

## 💡 Key Insights from the Dashboard

### Business Metrics (2017-2020):
- **Total Revenue:** ₹984.87M (normalized from USD to INR)
- **Total Sales Quantity:** 2.43M units
- **Markets Analyzed:** 15 (excluding international markets)
- **Top Market:** Delhi NCR contributing **52.8%** of total revenue

### Insights:
1. **Revenue Trend:** Declining sales from 2018 onwards, indicating market challenges
2. **Customer Concentration:** Top 5 customers generate **38%** of revenue
3. **Product Performance:** "Own Brand" products outperform distribution items
4. **Seasonal Patterns:** Q4 typically shows higher sales volume

---

## 🚀 Setup Instructions

### Prerequisites
- MySQL 8.0 or higher
- Tableau Desktop 2024.x
- 12 MB of disk space for the database

### Database Setup
1. **Install MySQL** and start the server
2. **Import the database:**
   ```bash
   mysql -u root -p < db_dump_version_2.sql
   ```
3. **Verify the import:**
   ```sql
   USE sales;
   SELECT COUNT(*) FROM transactions;  -- Should return 150,283 rows
   ```

### Tableau Configuration
1. Open `Sales Insights For AtliQ.twb` in Tableau Desktop
2. **Reconnect to your MySQL database:**
   - Server: `localhost`
   - Port: `3306`
   - Database: `sales`
   - Username: `root` (or your MySQL user)
3. The dashboard will automatically refresh with your data

---

## 📊 Dashboard Features

### Interactive Filters
- **Year Selector:** Filter data by specific years (2017-2020)
- **Month Selector:** Drill down to monthly performance
- **Cross-Filtering:** Click on any chart to filter all other visuals

### Visualizations
1. **Key Metrics Cards:** Revenue and Sales Quantity at a glance
2. **Revenue Trend Line:** Year-over-year performance
3. **Market Performance:** Bar chart showing top revenue-generating cities
4. **Top Customers & Products:** Identify key contributors
5. **Sales Quantity by Market:** Understand product distribution

---

## 🔧 Technical Highlights

### Currency Normalization
The dataset contains mixed currencies (INR and USD). A calculated field handles conversion:
```tableau
Normalized Amount = IF [currency] == 'USD' THEN [sales_amount] * 87.47 ELSE [sales_amount] END
```
*Note: Uses a fixed exchange rate. For production, consider dynamic rate tables.*

### Data Quality Filters
- Excluded markets: `Mark097` (New York), `Mark999` (Paris) - outside India scope
- Sales amount filter: `>= ₹1` to remove data quality issues

---

## 📈 Use Cases

This dashboard answers critical business questions:
- *"Which markets are underperforming this quarter?"*
- *"Who are our top 5 customers, and what are they buying?"*
- *"How did COVID-19 impact our 2020 sales?"*
- *"Should we focus more on Brick & Mortar or E-Commerce channels?"*

---

## 📝 Sample Queries

Explore the data directly with SQL:

```sql
-- Top 5 revenue-generating markets
SELECT m.markets_name, SUM(t.sales_amount) AS revenue
FROM transactions t
JOIN markets m ON t.market_code = m.markets_code
WHERE m.zone != ''
GROUP BY m.markets_name
ORDER BY revenue DESC
LIMIT 5;

-- Monthly revenue trend for 2020
SELECT d.month_name, SUM(t.sales_amount) AS revenue
FROM transactions t
JOIN date d ON t.order_date = d.date
WHERE d.year = 2020 AND t.currency = 'INR'
GROUP BY d.month_name
ORDER BY d.cy_date;
```

---

## 🤝 Contributing

This is a portfolio project, but suggestions are welcome! Feel free to:
- Open an issue for bugs or improvements
- Fork and experiment with the dashboard
- Share insights you discover in the data

---

## 📧 Contact

**Sourish Chatterjee**  
Data Analyst | Business Intelligence Enthusiast

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://linkedin.com/in/yourprofile)
[![Portfolio](https://img.shields.io/badge/Portfolio-FF5722?style=flat&logo=google-chrome&logoColor=white)](https://yourportfolio.com)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

**⭐ If you found this project useful, consider starring the repository!**
