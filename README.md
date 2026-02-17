# 📚 Smart Bookstore Data Analytics System (MySQL)

## 📌 Project Description
This project is a MySQL-based data analytics system designed to manage, retrieve, and analyze bookstore data using a large books dataset. It demonstrates SQL querying, data aggregation, normalization, and analytical reporting using relational database design.

---

## 🎯 Objective
To design and implement a structured database system that supports:
- Data filtering and retrieval
- Aggregation and reporting
- Schema normalization
- Analytical views for dashboards

---

## 🗄️ Database Tables
• books — stores book details  
• publishers — stores publisher information  
• categories — stores book categories  

---

## ⚙️ Features Implemented
✔ Data filtering by price, category, and rating  
✔ Aggregation reports (average price, reviews, ratings)  
✔ Advanced SQL queries for analytics  
✔ Schema normalization with foreign keys  
✔ Safe update and delete operations  
✔ Analytical views for reporting  

---

## 🧠 Key SQL Concepts Used
- SELECT, WHERE, ORDER BY  
- GROUP BY, Aggregation Functions  
- CASE Statements  
- Subqueries  
- Views  
- Foreign Keys  
- Database Normalization  

---

## 📈 Reporting Views (Dashboard Ready)

```sql
CREATE OR REPLACE VIEW top_rated_books AS
SELECT title, stars, price_incl_tax, category
FROM books
WHERE stars >= 4
ORDER BY stars DESC;

CREATE OR REPLACE VIEW category_summary AS
SELECT category,
       COUNT(*) AS total_books,
       AVG(price_incl_tax) AS avg_price,
       SUM(availability) AS total_stock
FROM books
GROUP BY category;
```
