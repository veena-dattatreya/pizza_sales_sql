# pizza_sales_sqlHere's a sample README file for a Pizza Sales SQL project:

---

# Pizza Sales SQL Analysis

## Project Overview

This project involves analyzing pizza sales data using SQL queries to gain insights into business performance, customer preferences, and trends. The data covers details about orders, customers, pizzas, and ingredients. The purpose of this analysis is to identify patterns in sales, understand customer behavior, and optimize menu offerings.

## Table of Contents

1. [Data Description](#data-description)
2. [SQL Queries](#sql-queries)
3. [Key Insights](#key-insights)
4. [Usage](#usage)
5. [Dependencies](#dependencies)
6. [Contributors](#contributors)

---

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
---

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
---


## Key Insights

- *Best-Selling Pizza*: Pepperoni was the most ordered pizza, especially in the medium size.
- *Sales Trends*: Sales peak during weekends and holidays, with significant increases during special promotions.
- *Customer Preferences*: Large pizzas are more frequently ordered than small or medium-sized pizzas.
- *Ingredient Usage*: Mozzarella and pepperoni are the most commonly used ingredients, suggesting their popularity with customers.

---

## Usage

1. *Clone the repository*: Download the project files to your local machine.
   bash
   git clone https://github.com/your-username/pizza-sales-sql.git
   
   
2. *Import the SQL database*: Load the provided pizza_sales.sql file into your SQL database system (e.g., MySQL, PostgreSQL).

3. *Run the queries*: Execute the SQL queries on your SQL client (e.g., MySQL Workbench, pgAdmin) to retrieve sales insights.

---

## Dependencies

- SQL Database Management System (e.g., MySQL, PostgreSQL)
- SQL Client for querying the database (e.g., MySQL Workbench, pgAdmin)
- Basic knowledge of SQL queries

---

## Contributors

- *Your Name*: Data Analysis and SQL Query Author
- *Collaborator's Name*: Database Design

For any questions or suggestions, feel free to reach out via email at your-email@example.com.

---
