# Case Study 1: Danny's Diner

## Table of Contents

[Entity Relationship Diagram](#entity-relationship-diagram)
[Datasets](#datasets)
[Questions \& Solution](#questions--solution)

## Entity Relationship Diagram

![dbdiagram](<Danny's Diner dbdiagram.png>)

## Datasets

```sql
CREATE TABLE sales (
  "customer_id" VARCHAR(1),
  "order_date" DATE,
  "product_id" INTEGER
);
GO

INSERT INTO sales
  ("customer_id", "order_date", "product_id")
VALUES
  ('A', '2021-01-01', '1'),
  ('A', '2021-01-01', '2'),
  ('A', '2021-01-07', '2'),
  ('A', '2021-01-10', '3'),
  ('A', '2021-01-11', '3'),
  ('A', '2021-01-11', '3'),
  ('B', '2021-01-01', '2'),
  ('B', '2021-01-02', '2'),
  ('B', '2021-01-04', '1'),
  ('B', '2021-01-11', '1'),
  ('B', '2021-01-16', '3'),
  ('B', '2021-02-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-01', '3'),
  ('C', '2021-01-07', '3');
GO

CREATE TABLE menu (
  "product_id" INTEGER,
  "product_name" VARCHAR(5),
  "price" INTEGER
);
GO

INSERT INTO menu
  ("product_id", "product_name", "price")
VALUES
  ('1', 'sushi', '10'),
  ('2', 'curry', '15'),
  ('3', 'ramen', '12');
GO

CREATE TABLE members (
  "customer_id" VARCHAR(1),
  "join_date" DATE
);
GO

INSERT INTO members
  ("customer_id", "join_date")
VALUES
  ('A', '2021-01-07'),
  ('B', '2021-01-09');
GO
```

## Questions & Solution

**1/ What is the total amount each customer spent at the restaurant?**

```sql
SELECT
sales.customer_id,
  SUM(menu.price) AS total_sales
FROM sales
INNER JOIN menu
  ON sales.product_id = menu.product_id
GROUP BY sales.customer_id
ORDER BY sales.customer_id ASC;
GO
```

**Answer:**

|     | customer_id | total_sales |
| --- | ----------- | ----------- |
| 1   | A           | 76          |
| 2   | B           | 74          |
| 3   | C           | 36          |

**2/ How many days has each customer visited the restaurant?**

```sql
SELECT
	customer_id,
	COUNT(DISTINCT order_date) AS visit_count
FROM sales
GROUP BY customer_id
GO
```

**Answer:**
| | customer_id | visit_count |
| --- | ----------- | ----------- |
| 1 | A | 4 |
| 2 | B | 6 |
| 3 | C | 2 |
