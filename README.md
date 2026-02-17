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

**Data Retrieval Queries**
-- Books priced above 50
SELECT title, price_incl_tax
FROM books
WHERE price_incl_tax > 50;

-- Books in 'Travel' category sorted by price
SELECT title, price_excl_tax
FROM books
WHERE category = 'Travel'
ORDER BY price_excl_tax DESC;

-- Books with 4+ stars and availability ≤ 10
SELECT title, stars, availability
FROM books
WHERE stars >= 4 AND availability <= 10;


**Data Aggregation & Analysis**
-- Total books & average price per category
SELECT category,
       COUNT(*) AS total_books,
       AVG(price_incl_tax) AS avg_price
FROM books
GROUP BY category;

-- Top 5 most-reviewed books
SELECT title, num_reviews
FROM books
ORDER BY num_reviews DESC
LIMIT 5;

-- Average star rating per category
SELECT category,
       AVG(stars) AS avg_stars
FROM books
GROUP BY category;

**Complex Queries & Reporting**

-- Classify books by price range
SELECT title, price_incl_tax,
CASE
    WHEN price_incl_tax > 40 THEN 'Expensive'
    WHEN price_incl_tax BETWEEN 20 AND 40 THEN 'Moderate'
    ELSE 'Affordable'
END AS price_level
FROM books;

-- Highest priced book in each category
SELECT category, title, price_incl_tax
FROM books b
WHERE price_incl_tax = (
    SELECT MAX(price_incl_tax)
    FROM books
    WHERE category = b.category
);

-- Percentage of total books per category
SELECT category,
       COUNT(*) AS book_count,
       ROUND((COUNT(*) * 100.0 / (SELECT COUNT(*) FROM books)), 2) AS percentage
FROM books
GROUP BY category;

**Data Maintenance & Updates**

-- Find a book’s ID
SELECT id, title
FROM books
WHERE title = 'The Great Adventure';

-- Safe update
UPDATE books
SET tax = 5.00,
    price_incl_tax = price_excl_tax + 5.00
WHERE id = 10;

-- Preview books to delete
SELECT id, title, availability
FROM books
WHERE availability = 0;

-- Safe delete
DELETE FROM books
WHERE id IN (
    SELECT id FROM (
        SELECT id FROM books WHERE availability = 0
    ) AS temp
);

**Schema Improvements (Normalization)**

-- Create Publisher Table
CREATE TABLE publishers (
    publisher_id INT AUTO_INCREMENT PRIMARY KEY,
    publisher_name VARCHAR(100),
    country VARCHAR(50)
);

-- Create Categories Table
CREATE TABLE categories (
    category_id INT AUTO_INCREMENT PRIMARY KEY,
    category_name VARCHAR(100)
);

-- Add Missing Columns
ALTER TABLE books ADD COLUMN publisher_id INT NULL;
ALTER TABLE books ADD COLUMN category_id INT NULL;

-- Add Foreign Keys
ALTER TABLE books
ADD CONSTRAINT fk_publisher
FOREIGN KEY (publisher_id) REFERENCES publishers(publisher_id),
ADD CONSTRAINT fk_category
FOREIGN KEY (category_id) REFERENCES categories(category_id);

**Reporting Views (Dashboard Ready)**

-- Top Rated Books View
CREATE OR REPLACE VIEW top_rated_books AS
SELECT title, stars, price_incl_tax, category
FROM books
WHERE stars >= 4
ORDER BY stars DESC;

-- Category Summary View
CREATE OR REPLACE VIEW category_summary AS
SELECT category,
       COUNT(*) AS total_books,
       AVG(price_incl_tax) AS avg_price,
       SUM(availability) AS total_stock
FROM books
GROUP BY category;
