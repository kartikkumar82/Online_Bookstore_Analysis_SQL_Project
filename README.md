# Online Bookstore Management System Project

## Project Overview

**Project Title**: Online Bookstore Management System  
**Level**: Beginner  
**Database**: `books_db, customers_db, orders_db`

An end-to-end database project for managing an online bookstore. It includes tables for books, customers, and orders, all interconnected using SQL queries. The system supports inventory tracking, order processing, and customer management.

## Objectives
🗃️ Manage book inventory (title, author, price, stock, etc.)
👤 Track customer details and purchase history
📦 Handle orders with date, status, and total cost
🔍 Perform SQL queries like joins, aggregations, filtering, and more
📈 Generate insights such as best-selling books, total revenue, etc.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `books_db, customers_db, orders_db`.


- **Table Creation**: A table named `Books, Customers, Orders` is created to store the sales data. The table structure includes columns for Book_ID, Title,	Author,	Genre,	Published_Year, Price, Stock in Books table and Customer_ID, Name, Email,	Phone, City, Country in Customers table and Order_ID, Customer_ID, Book_ID,	Order_Date,	Quantity, Total_Amount in Orders table.


```sql
CREATE DATABASE online_bookstore;

-- Create Tables
DROP TABLE IF EXISTS Books;
CREATE TABLE Books (
    Book_ID SERIAL PRIMARY KEY,
    Title VARCHAR(100),
    Author VARCHAR(100),
    Genre VARCHAR(50),
    Published_Year INT,
    Price NUMERIC(10, 2),
    Stock INT
);
DROP TABLE IF EXISTS customers;
CREATE TABLE Customers (
    Customer_ID SERIAL PRIMARY KEY,
    Name VARCHAR(100),
    Email VARCHAR(100),
    Phone VARCHAR(15),
    City VARCHAR(50),
    Country VARCHAR(150)
);
DROP TABLE IF EXISTS orders;
CREATE TABLE Orders (
    Order_ID SERIAL PRIMARY KEY,
    Customer_ID INT REFERENCES Customers(Customer_ID),
    Book_ID INT REFERENCES Books(Book_ID),
    Order_Date DATE,
    Quantity INT,
    Total_Amount NUMERIC(10, 2)
);

```

### 2. Data Import

```sql
COPY Books(Book_ID, Title, Author, Genre, Published_Year, Price, Stock)
FROM 'D:\POSTGRE SQL\ST - SQL ALL PRACTICE FILES\All Excel Practice Files\Books.csv'
CSV HEADER;

COPY customers (customer_id, name, email, phone, city, country)
FROM 'D:\POSTGRE SQL\ST - SQL ALL PRACTICE FILES\All Excel Practice Files\Customers.csv'
CSV HEADER;

COPY orders(order_id, customer_id, book_id, order_date, quantity, total_amount)
FROM 'D:\POSTGRE SQL\ST - SQL ALL PRACTICE FILES\All Excel Practice Files\Orders.csv'
CSV HEADER;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Retrieve all books in the "Fiction" genre**:
```sql
SELECT *
FROM books
WHERE genre = 'Fiction';
```

2. **Find books published after the year 1950**:
```sql
SELECT *
FROM books
WHERE published_year > 1950;
```

3. **Show orders placed in November 2023.**:
```sql
SELECT *
FROM orders
WHERE order_date BETWEEN '2023-01-01' AND '2023-11-30';
```

4. **Retrieve the total number of books sold for each genre.**:
```sql
SELECT *
FROM books;
SELECT b.genre, SUM(o.quantity) AS total_quantity_sold
FROM books b
JOIN orders o
ON b.book_id = o.book_id
GROUP BY genre;
```

5. **List customers who have placed at least 2 orders.**:
```sql
SELECT o.customer_id, c.name, COUNT(o.quantity) AS order_count
FROM Customers c
JOIN orders o
ON c.customer_id = o.customer_id
GROUP BY o.customer_id, c.name
HAVING COUNT(o.quantity)>2;
```

6. **Find the most frequently ordered book.**:
```sql
SELECT
o.Book_id, b.title, COUNT(o.order_id) AS ORDER_COUNT
FROM
orders o
JOIN books b
ON o.book_id=b.book_id
GROUP BY o.book_id, b.title
ORDER BY ORDER_COUNT DESC
LIMIT 1;
```

7. **Find the customer who spent the most on orders**:
```sql
SELECT
c.customer_id, c.name, SUM(o.total_amount) AS total_spend
FROM
orders o
JOIN customers c
ON c.customer_id = o.order_id
GROUP BY c.customer_id, c.name
ORDER BY total_spend DESC 
LIMIT 1;
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales**:
```sql
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```

9. **List the cities where customers who spent over 30 are located.**:
```sql
SELECT
DISTINCT c.city, o.total_amount
FROM orders o
JOIN customers c
ON o.customer_id=c.customer_id
WHERE o.total_amount > 30;
```

10. **Calculate the stock remaining after fulfilling all orders**:
```sql
SELECT
b.title, b.book_id,b.stock,
COALESCE(SUM(o.quantity),0)
AS Order_quantity,
  b.stock - COALESCE(SUM(o.quantity),0)
FROM books b
LEFT JOIN orders o
ON b.book_id = o.book_id
GROUP BY b.book_id
ORDER BY book_id ASC;
```


## Reports

- **Books Summary**: A detailed report summarizing total books, author, and publisher year.
- **Orders Analysis**: Insights into orders trends across different books and customers.
- **Customer Insights**: Reports on top customers and unique customer counts per order.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data importing, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

## How to Use

1. **Clone the Repository**: Clone this project repository from GitHub.
2. **Set Up the Database**: Run the SQL scripts provided in the `database_setup.sql` file to create and populate the database.
3. **Run the Queries**: Use the SQL queries provided in the `analysis_queries.sql` file to perform your analysis.
4. **Explore and Modify**: Feel free to modify the queries to explore different aspects of the dataset or answer additional business questions.

## Author - Kartik Kumar

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. If you have any questions, feedback, or would like to collaborate, feel free to get in touch!

**E-mail** : kartikkumar1800089@gmail.com


Thank you for your support, and I look forward to connecting with you!
