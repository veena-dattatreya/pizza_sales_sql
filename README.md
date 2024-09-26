

# Pizza Sales SQL Analysis

## Project Overview

This project involves analyzing pizza sales data using SQL queries to gain insights into business performance, customer  and trends. The data covers details about orders,  pizzas, and ingredients. The purpose of this analysis is to identify patterns in sales.

## Table of Contents

1. [Data Description](#data-description)
2. [SQL Queries](#sql-queries)
3. [Key Insights](#key-insights)


## Data Description

The database consists of several tables related to pizza sales, including:

- *Orders*: Contains information on each customer order, including order ID, customer ID, date, and total amount.
- *Pizzas*: Details of each pizza available for sale, including pizza ID, name, and size.
- *Pizza_types*: A breakdown of the ingredients used in each pizza.
- *Order_Details*: Contains information about the specific pizzas ordered, including order ID, pizza ID, quantity, and price.

### Sample Table 

-orders

-order details

-pizza types

-pizzas

---

## SQL Queries

The following are some sample SQL queries used in the analysis:

### 1.create tables
```sql
drop table if exists order_details;
create table (order_details_id int,
order_id int,
pizza_id text,
quantity int);

drop table if exists order;
create table (order_id int,
order_date date,
pizza_time time,
);

drop table if exists pizza_types;
create table pizza_types
(pizza_type_id varchar(50),name varchar(50),category varchar(100),ingredients varchar(250));

DROP TABLE IF EXISTS pizzas;

CREATE TABLE IF NOT EXISTS pizzas
(
    pizza_id varchar(25),
    pizza_type_id varchar(50),
    size varchar(5),
    price double;
)
```


### 2. Retrieve the total number of orders placed.
```sql
select count(order_id)as total_orders from orders
```

### 3.Calculate the total revenue generated from pizza sales.
```sql
select 
    round(sum(order_details.quantity*pizzas.price))as total_revenue
from 
    order_details 
	  inner join 
	  pizzas
      on order_details.pizza_id=pizzas.pizza_id
```

### 4.Identify the highest-priced pizza.
```sql
SELECT 
    pizza_types.name, pizzas.price
FROM
    pizzas
        JOIN
    pizza_types ON pizzas.pizza_type_id = pizza_types.pizza_type_id
ORDER BY price DESC
LIMIT 1
```

### 5. Identify the most common pizza size ordered.

```sql
SELECT 
    COUNT(order_details.order_details_id), pizzas.size
FROM
    pizzas
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizzas.size
ORDER BY 1 DESC
LIMIT 1
```

### 6.List the top 5 most ordered pizza types along with their quantities.

```sql
SELECT 
    COUNT(order_details.order_details_id), pizzas.size
FROM
    pizzas
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY pizzas.size
ORDER BY 1 DESC
LIMIT 1
```
### 7.Join the necessary tables to find the total quantity of each pizza category ordered.

```sql
SELECT 
    SUM(order_details.quantity) AS total_quantity,
    pizza_types.category
FROM
    pizza_types
        JOIN
    pizzas ON pizza_types.pizza_type_id = pizzas.pizza_type_id
        JOIN
    order_details ON pizzas.pizza_id = order_details.pizza_id
GROUP BY 2
ORDER BY 1 DESC

```

### 8.Determine the distribution of orders by hour of the day.

```sql
select
     extract (hour from order_time),count(order_id) from orders
group by 1
order by 1
```

### 9.Join relevant tables to find the category-wise distribution of pizzas.
```sql
SELECT 
    category, COUNT(name) AS pizzas
FROM
    pizza_types
GROUP BY 1
```
### 10.Group the orders by date and calculate the average number of pizzas ordered per day.
```sql
with cte as
(
SELECT 
    orders.order_date, SUM(order_details.quantity) AS quantity
FROM
    orders
        JOIN
    order_details ON orders.order_id = order_details.order_id
GROUP BY 1
ORDER BY 1
)
select round(avg(quantity)) from cte
```
### 11.Determine the top 3 most orderd pizza types based on the revenue
```sql
SELECT 
    pt.name, SUM(p.price * od.quantity) AS revenue
FROM
    pizzas p
        JOIN
    pizza_types pt ON p.pizza_type_id = pt.pizza_type_id
        JOIN
    order_details od ON p.pizza_id = od.pizza_id
GROUP BY 1
ORDER BY 2 DESC
LIMIT 3
```
### 12.Calculate the percentage contribution of each pizza type to total revenue.with total_revenue as
```sql
select pt.category,round((round(sum(od.quantity*p.price))::numeric/(select 
    round(sum(od.quantity*p.price))
from 
    order_details od 
	  inner join 
	  pizzas p
      on od.pizza_id=p.pizza_id))*100) as
total_revenue from pizzas p
join
pizza_types pt
on p.pizza_type_id=pt.pizza_type_id
join order_details od
on od.pizza_id=p.pizza_id
group by 1
```


### 13.Analyze the cumulative revenue generated over time.

```sql
with sales as(
select orders.order_date,
sum(order_details.quantity*pizzas.price)as revenue
from orders join order_details
on orders.order_id=order_details.order_id
join
pizzas on pizzas.pizza_id=order_details.pizza_id
group by 1 )

select order_date,revenue,sum(revenue)  over(order by order_date) as cumulative_revenue from sales
```
### 14.Determine the top 3 most ordered pizza types based on revenue for each pizza category.
```sql
with new_pizza as(
select pizza_types.category,pizza_types.name, sum(pizzas.price*order_details.quantity)as rev
from
pizza_types join pizzas on pizza_types.pizza_type_id=pizzas.pizza_type_id
join order_details 
 on order_details.pizza_id=pizzas.pizza_id
group by 1,2)
select category,name,rev from
(select category,name,rev,dense_rank() over(partition by category order by rev desc) as rnk from
new_pizza)
where rnk<=3
```

## Key Insights

- *Best-Selling Pizza*:  most ordered pizza, especially in the category,size.
- *Sales Trends*: cumulative Sales 
---



## Dependencies

- SQL Database Management System (e.g., MySQL, PostgreSQL)
- SQL Client for querying the database (e.g., MySQL Workbench, pgAdmin)
- Basic knowledge of SQL queries

---

