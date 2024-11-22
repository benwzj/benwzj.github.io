---
layout: post
title: Joins and Subqueries in SQL
date: 2024-11-21
category: Database
tags: SQL Database
---

## Understand Joins

Joins clause in SQL used to combine records from two or more tables. These tables are linked through common fields, and a join operation fetches data based on specified conditions. 

Joins can be of various types. Here’s a brief overview:

- INNER JOIN: Returns only the rows that have matching values in both tables.
- OUTER JOIN: Returns all rows from both tables, including unmatched rows.
- LEFT(OUTER) JOIN: Returns all records from the left table, and the matched records from the right table
- RIGHT(OUTER) JOIN: Returns all records from the right table, and the matched records from the left table
- CROSS JOIN: Produces a Cartesian product of the two tables, resulting in a combination of all rows.

This Figures do a good job on showing the difference:
{% include figure.html path="assets/img/sql-joins.webp" class="img-fluid rounded z-depth-1" width="80%" %}

Joins are specified in the FROM clause of a query and can significantly enhance the efficiency of data retrieval. They are essential for fetching data from multiple tables based on relationships.

Usually we just use INNER JOIN and LEFT(OUTER) JOIN.

### The INNER JOIN usually can be written without JOIN keyword: 
```sql
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
FROM Orders
INNER JOIN Customers ON Orders.CustomerID=Customers.CustomerID;
```
same with:
```sql
SELECT Orders.OrderID, Customers.CustomerName, Orders.OrderDate
FROM Orders, Customers
where Orders.CustomerID=Customers.CustomerID;
```

### Understand Full Join

Full join means list all rows in tow table, if the rows meet condition, then join them together.
But some popular DB like MySQL does not support the FULL OUTER JOIN syntax.

### What is self join
A self join is a regular join, but the table is joined with itself.

The following SQL statement matches customers that are from the same city:
```sql
SELECT A.CustomerName AS CustomerName1, B.CustomerName AS CustomerName2, A.City
FROM Customers A, Customers B
WHERE A.CustomerID <> B.CustomerID
AND A.City = B.City
ORDER BY A.City;
```


## What is a Subquery

A Subquery, also known as an Inner query or Nested query, is a query nested within another SQL query. It is enclosed within the WHERE clause and serves various purposes, such as filtering rows, calculating values, or retrieving data conditionally. Subqueries can be used in conjunction with SELECT, INSERT, UPDATE, DELETE, or CREATE VIEW statements.

## JOINs vs Subqueries

Usually, Subqueries are the logically correct way to solve problems. Subqueries are more readable than JOINs. However, it always comes down to performance. Although optimisers are getting better, in most cases JOINs are faster than subqueries.

Important factors:
- There are different DBMSs
- Size matters
- There are different forms of sub-queries

Historically, explicit joins usually win, hence the established wisdom that joins are better, but optimisers are getting better all the time, and so I prefer to write queries first in a logically coherent way, and then restructure if performance constraints warrant this.

### How DBMS process SQL query

The journey of an SQL query starts with parsing and tokenization, where the query is broken down into individual elements such as keywords (e.g., SELECT, FROM, WHERE) and operators (e.g., =, >, <). Following this, the database management system (DBMS) analyzes the query to devise optimal execution plans for data retrieval. With the execution plan in place, the DBMS begins the process of data retrieval. If the query involves multiple tables, the DBMS performs join operations to combine the relevant data. Subsequently, filtering conditions specified in the WHERE clause are applied to assess each row’s eligibility based on user-defined criteria. Additionally, common aggregation functions like SUM, MIN, MAX, AVG are utilized to perform calculations on grouped data. Upon completion of these operations, the DBMS generates the final result set, culminating the query execution process.

{% include figure.html path="assets/img/sql-process.webp" class="img-fluid rounded z-depth-1" width="60%" %}

Join operations are seamlessly integrated within the query execution steps. However, when using a subquery, the process involves executing the entire inner query first. Afterward, the table generated from this query is utilized in the WHERE clause to execute the outer query. This fundamental difference underscores why joins are favored over subqueries, particularly when dealing with large volumes of data. Sometimes, though, subqueries are preferred over joins when dealing with smaller datasets or when the complexity of the data requires it, mainly because they offer easier readability.

## References

- w3schools: [SQL Join Doc](https://www.w3schools.com/sql/sql_join.asp)
- stackoverflow topic: [join-vs-sub-query](https://stackoverflow.com/questions/2577174/join-vs-sub-query)