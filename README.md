# 🍕 Pizza Sales Analysis - SQL Project

A comprehensive SQL database project analyzing pizza sales data from a restaurant's complete year of transactions (2015). This project demonstrates database design, data analysis, and business intelligence capabilities using MySQL.

## 📊 Project Overview

This project contains real transactional data from a pizza restaurant, including:
- **21,350** total orders
- **48,620** individual pizza items sold
- **32** different pizza types
- **4** pizza categories
- Complete pricing and ingredient information

## 🗂️ Database Schema

### Tables Structure

#### 1. **orders**
Stores main order information
- `order_id` (Primary Key) - Unique order identifier
- `date` - Order date (YYYY-MM-DD)
- `time` - Order time (HH:MM:SS)

**Records:** 21,350 orders

#### 2. **order_details**
Contains line items for each order
- `order_details_id` (Primary Key) - Unique line item ID
- `order_id` (Foreign Key) - Links to orders table
- `pizza_id` (Foreign Key) - Links to pizzas table
- `quantity` - Number of pizzas ordered

**Records:** 48,620 line items

#### 3. **pizzas**
Pizza variants with pricing
- `pizza_id` (Primary Key) - Unique pizza variant ID
- `pizza_type_id` (Foreign Key) - Links to pizza_types table
- `size` - Pizza size (S, M, L)
- `price` - Price in USD

**Records:** ~97 pizza variants

#### 4. **pizza_types**
Master pizza type information
- `pizza_type_id` (Primary Key) - Unique pizza type ID
- `name` - Full pizza name
- `category` - Pizza category (Chicken, Classic, Supreme, Veggie)
- `ingredients` - Comma-separated ingredient list

**Records:** 32 pizza types

### 🔗 Relationships
```
orders (1) ──→ (N) order_details
pizza_types (1) ──→ (N) pizzas
pizzas (1) ──→ (N) order_details
```

## 📁 Project Files

- **`ducatsolu.mysql.mwb`** - MySQL Workbench model file (main database design)
- **`orders.csv`** - Order transaction data
- **`order_details.csv`** - Order line items
- **`pizzas.csv`** - Pizza variants and pricing
- **`pizza_types.csv`** - Pizza types, categories, and ingredients
- **`trigger/ducatsolu.mysql.mwb`** - Additional database model

## 🚀 Getting Started

### Prerequisites
- MySQL Server (5.7 or higher)
- MySQL Workbench (optional, for viewing .mwb files)

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/rajrahul19/pizzae-sales.git
   cd pizzae-sales
   ```

2. **Create Database**
   ```sql
   CREATE DATABASE pizza_sales;
   USE pizza_sales;
   ```

3. **Create Tables**
   ```sql
   CREATE TABLE orders (
       order_id INT PRIMARY KEY,
       date DATE,
       time TIME
   );

   CREATE TABLE pizza_types (
       pizza_type_id VARCHAR(50) PRIMARY KEY,
       name VARCHAR(100),
       category VARCHAR(50),
       ingredients TEXT
   );

   CREATE TABLE pizzas (
       pizza_id VARCHAR(50) PRIMARY KEY,
       pizza_type_id VARCHAR(50),
       size CHAR(1),
       price DECIMAL(5,2),
       FOREIGN KEY (pizza_type_id) REFERENCES pizza_types(pizza_type_id)
   );

   CREATE TABLE order_details (
       order_details_id INT PRIMARY KEY,
       order_id INT,
       pizza_id VARCHAR(50),
       quantity INT,
       FOREIGN KEY (order_id) REFERENCES orders(order_id),
       FOREIGN KEY (pizza_id) REFERENCES pizzas(pizza_id)
   );
   ```

4. **Import CSV Data**
   - Use MySQL Workbench's import wizard, or
   - Use `LOAD DATA INFILE` command for each CSV file

## 📈 Sample Analysis Queries

### 1. Total Revenue
```sql
SELECT ROUND(SUM(od.quantity * p.price), 2) AS total_revenue
FROM order_details od
JOIN pizzas p ON od.pizza_id = p.pizza_id;
```

### 2. Most Popular Pizza
```sql
SELECT pt.name, SUM(od.quantity) AS total_ordered
FROM order_details od
JOIN pizzas p ON od.pizza_id = p.pizza_id
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.name
ORDER BY total_ordered DESC
LIMIT 5;
```

### 3. Sales by Category
```sql
SELECT pt.category, 
       COUNT(DISTINCT od.order_id) AS orders,
       SUM(od.quantity) AS pizzas_sold,
       ROUND(SUM(od.quantity * p.price), 2) AS revenue
FROM order_details od
JOIN pizzas p ON od.pizza_id = p.pizza_id
JOIN pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
GROUP BY pt.category
ORDER BY revenue DESC;
```

### 4. Peak Order Times
```sql
SELECT HOUR(time) AS hour, COUNT(*) AS order_count
FROM orders
GROUP BY HOUR(time)
ORDER BY order_count DESC;
```

### 5. Average Order Value
```sql
SELECT ROUND(AVG(order_total), 2) AS avg_order_value
FROM (
    SELECT o.order_id, SUM(od.quantity * p.price) AS order_total
    FROM orders o
    JOIN order_details od ON o.order_id = od.order_id
    JOIN pizzas p ON od.pizza_id = p.pizza_id
    GROUP BY o.order_id
) AS order_totals;
```

## 💡 Analysis Opportunities

This dataset enables various analyses:
- **Revenue Analysis** - Daily, monthly, and yearly trends
- **Product Performance** - Best and worst-selling pizzas
- **Customer Behavior** - Order patterns, peak hours
- **Size Preferences** - Small vs Medium vs Large popularity
- **Category Analysis** - Chicken vs Classic vs Supreme vs Veggie
- **Ingredient Analysis** - Most common ingredients
- **Pricing Strategy** - Price optimization by size/type

## 📊 Key Statistics

- **Data Period:** January 1, 2015 - December 31, 2015
- **Total Orders:** 21,350
- **Total Pizzas Sold:** 48,620
- **Average Pizzas per Order:** ~2.3
- **Pizza Categories:** 4 (Chicken, Classic, Supreme, Veggie)
- **Size Options:** 3 (Small, Medium, Large)
- **Price Range:** $12.75 - $35.95

## 🛠️ Technologies Used

- **MySQL** - Database management
- **MySQL Workbench** - Database design and modeling
- **CSV** - Data storage format

## 👤 Author

**Rahul Raj**
- GitHub: [@rajrahul19](https://github.com/rajrahul19)
- Email: rahulraj18245@gmail.com

## 📝 License

This project is open source and available for educational purposes.

## 🤝 Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the issues page.

---

⭐ If you found this project helpful, please consider giving it a star!
