# SQL Learning Notes

## Window Functions

### ROW_NUMBER()

```sql
SELECT 
    name,
    department,
    salary,
    ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) as rank
FROM employees;
```

### LAG() and LEAD()

```sql
SELECT 
    date,
    revenue,
    LAG(revenue, 1) OVER (ORDER BY date) as prev_day,
    LEAD(revenue, 1) OVER (ORDER BY date) as next_day
FROM daily_sales;
```

### SUM() OVER

```sql
SELECT 
    date,
    sales,
    SUM(sales) OVER (ORDER BY date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) as running_total
FROM daily_sales;
```

---

## CTEs (Common Table Expressions)

```sql
WITH monthly_sales AS (
    SELECT 
        DATE_TRUNC('month', order_date) as month,
        SUM(amount) as total_sales
    FROM orders
    GROUP BY 1
)
SELECT 
    month,
    total_sales,
    total_sales - LAG(total_sales) OVER (ORDER BY month) as growth
FROM monthly_sales;
```

---

## Aggregate Functions

```sql
-- Basic aggregates
SELECT 
    COUNT(*) as total_orders,
    SUM(amount) as total_revenue,
    AVG(amount) as avg_order_value,
    MIN(amount) as min_order,
    MAX(amount) as max_order
FROM orders;

-- Group by
SELECT 
    category,
    COUNT(*) as product_count,
    AVG(price) as avg_price
FROM products
GROUP BY category
HAVING COUNT(*) > 10;
```

---

## Joins

```sql
-- Inner Join
SELECT 
    o.id,
    o.order_date,
    c.name as customer_name
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id;

-- Left Join
SELECT 
    c.name,
    COUNT(o.id) as order_count
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id
GROUP BY c.name;

-- Multiple Joins
SELECT 
    o.id,
    c.name as customer,
    p.name as product,
    oi.quantity
FROM orders o
JOIN customers c ON o.customer_id = c.id
JOIN order_items oi ON o.id = oi.order_id
JOIN products p ON oi.product_id = p.id;
```

---

## Subqueries

```sql
-- In WHERE
SELECT * FROM products
WHERE category_id IN (
    SELECT id FROM categories WHERE active = true
);

-- In SELECT
SELECT 
    name,
    price,
    (SELECT AVG(price) FROM products) as avg_price
FROM products;

-- Correlated Subquery
SELECT 
    name,
    salary,
    (SELECT AVG(salary) FROM employees e2 WHERE e2.department = e1.department) as dept_avg
FROM employees e1;
```
