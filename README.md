#MySQL

MySQL is a popular open-source relational database management system 🎉. It organizes data into tables using SQL, making it great for web applications. With cross-platform compatibility and robust security features 🔒, MySQL is a top choice for developers. Its scalability suits both small projects and large enterprises 🚀. The active community provides excellent support, and features like InnoDB for transactions and replication for backup enhance its reliability 💪. Whether you're a beginner or an expert, MySQL has you covered!

---

```sql
USE sql_store;

SELECT * 
FROM customers
WHERE customer_id = 1
ORDER BY first_name
```
--- 

# `SHIFT + CMD + ENTER`. ⇒ to run the queries

---

# SELECT Clause

```sql
SELECT 
     last_name,
     first_name,
     points,
     (points + 10) * 100 AS discount_factor
FROM customers
```
--- 

```sql
SELECT  DISTINCT state
FROM customers
```

---

# WHERE Clause

```sql
SELECT *
FROM Customers
WHERE state <> 'va'
--select all from customers where state is not equal to 'va' can also write !=
```

---

```sql
SELECT * FROM orders WHERE order_date >= "2019-01-01"
```

---

# AND | OR | NOT Operators

```sql
SELECT * FROM Customers WHERE birth_data > '1990-01-01' OR points > 1000 AND state = 'va'
```

---

```sql
SELECT * FROM order_items WHERE order_id = 6 AND (quantity * unit_price) > 30
```

---

```sql
SELECT * FROM Customers WHERE state IN ('va', 'fl', 'ga')
```

---

```sql
SELECT * FROM Customers WHERE state NOT IN ('va', 'fl', 'ga')
```
---

### `BETWEEN`

```sql
SELECT * FROM Customers WHERE points BETWEEN 1000 AND 3000
```

---

```sql
SELECT * FROM customers WHERE last_name LIKE 'b%'
-- last_name start with b and any number of character after => %
```

---

```sql
SELECT * FROM customers WHERE last_name LIKE '%b%'
-- last_name have b alphabet. doesnot matter first or last place
```

---

```sql
SELECT * FROM customers WHERE last_name LIKE '%b'
--last_name have b in the last.
```

we use _ to represent single character and % to represend any amount of character.

---

# REGEX

```sql
SELECT * FROM customers WHERE last_name REGEXP '[a-h]'
-- ^ => beginning
-- $ => end
-- | => Logical OR
-- [abcd] => match any character
-- [a-f] => match any from range
```

---

```sql
SELECT * FROM customers WHERE last_name REGEXP 'filed$' 
```

---

```sql
SELECT * FROM customers WHERE last_name REGEXP '^field|mac|rose'
```

---

```sql
SELECT * FROM customers WHERE last_name REGEXP '[gim]e'
```

---

# IS NULL

```sql
SELECT * FROM customers WHERE phone IS NULL
```

---

# ORDER BY

```sql
SELECT * FROM customers ORDER BY first_name
```

---

```sql
SELECT * FROM customers ORDER BY first_name DESC
```

---

```sql
SELECT * FROM customers ORDER BY state, first_name
```

---

```sql
SELECT * FROM order_items WHERE order_id = 2 ORDER BY quantity * unit_price DESC
```

# LIMIT CLAUSE

```sql
SELECT * FROM customers LIMIT 3
--SELECT ALL FROM CUSTOMERS BUT SHOW ONLY FIRST 3
```

---

```sql
SELECT * FROM customers LIMIT 6, 3
-- select all from customers and skip first 6 and show 3 customers after that. 6=> offset
```

```sql
SELECT * FROM customers ORDER BY points DESC LIMIT 3
-- select all from customers and show them by point in descending order but 
-- only show first 3. here order is imp. limit should come at last.

```

---

# INNER JOIN

```sql
SELECT * 
FROM orders
JOIN customers 
    ON orders.customer_id = customers.customer_id

--select all from the orders table and join customers table ON the basis of 
-- orders.customer_id = customers.customer_id
```

---

```sql
SELECT order_id, first_name, last_name
FROM orders
JOIN customers 
    ON orders.customer_id = customers.customer_id

-- in first query, it will show all the column from order and all from customers
-- to limit the column, we can say SELECT order_id, first_name , last_name
-- note that, if the column is in both the table like customer_id, we need to 
-- prefix one of the table like => orders.customer_id. know that value will be same

```

---

```sql
SELECT order_id, oi.product_id, quantity, oi.unit_price
FROM order_items oi
join products p
ON oi.product_id = p.product_id

--here to escape from repeating order_items, we keep the alias oi for order_items
-- and p for products.
```

# JOINING OUTER TABLE FROM DATABASE

```sql
USE sql_store;
SELECT * FROM order_items oi
JOIN sql.inventory.products p
     ON oi.product_id = p.product_id

--we need to specify which database in order to join outer database
```

---

# SELF JOINS

```sql
USE sql_hr;

SELECT 
     e.employee_id,
     e.first_name,
     m.first_name AS manager
FROM employees e
JOIN employees m
    ON e.reports_to = m.employee_id

-- for the self joins we need to provide different allias for the same employee 
-- and infer allias to every column
```

# JOINING MULTIPLE TABLES

```sql
USE sql_store;

SELECT 
   o.order_id,
   o.order_date,
   c.first_name,
   c.last_name,
   os.name AS status
FROM orders o
JOIN customers c
     ON o.customer_id = c.customer_id
JOIN order_statuses os
      ON o.status = os.order_status_id
```

---

# Compound Join Conditions

```sql
use sql_store;

SELECT * 
FROM order_items oi
JOIN order_item_notes oin
    ON oi.order_id = oin.order_id
    AND oi.product_id = oin.product_id
```

# Implicit Join Syntax

```sql
SELECT *
FROM orders o, customers c
WHERE o.customer_id = c.customer_id

--this is implicit join and it is good practise to use join => explicit
```

---

# OUTER JOIN

```sql
SELECT 
  c.customer_id,
  c.first_name,
  o.order_id
FROM customers c
LEFT JOIN orders o
  ON c.customer_id = o.customer_id
ORDER BY c.customer_id
--by indicating left join we are saying join all customers whether they meet ON 
-- condition or not but only join orders when they meet conditions
--when we use right, it is vice versa as left.

```
