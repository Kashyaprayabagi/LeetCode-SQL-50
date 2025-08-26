
# Leet Code - SQL 50 

Basics of SQL and solutions to all LeetCode SQL50 will be available in this file.




## 1. Recyclable and Low Fat Products


The Solution for this is to find all the products that have low fats and are recylable. We need to return their IDs.

```bash
  SELECT product_id FROM Products
  WHERE low_fats = 'Y' AND recyclable = 'Y';
```

The AND operator is used here.

## 2. Find Customer Referee


The Solution for this is to find the names of Customer where referee_id is not equal to 2. 

```bash
  SELECT name FROM Customer
  WHERE referee_id != 2 or referee_id IS NULL;    
```

NOTE: SQL **do not** return values with **NULL** operator by default. Hence we need to specify it seperately.

## 3. Big Countries


The Solution for this is to find the names, population and area of countries that either has area >= 3000000 or a population >= 25000000.
```bash
  SELECT name, population, area
  FROM World WHERE 
  area >= 3000000 OR population >= 25000000;    
```

NOTE: **OR** operator is used to do the same.

## 4. Article Views I


The Solution for this is to find the tuples that have same author_id and viewer_id. However their might be many authors who read their own articles atleast once..so we are asked to return the unique values of the author_id.
```bash
  SELECT DISTINCT author_id AS id 
  FROM Views WHERE author_id = viewer_id
  ORDER BY id;   
```

NOTE: **DISTINCT** keyword will return only unique values. **Specify the coloumn_name after ORDER BY clause.** **ASC** or **DESC** can be written later.

## 5. Invalid Tweets (CHAR_LENGTH)


The challenge in this question is to find the lenght of the string. There are 2 methods to do it **LENGTH** and **CHAR_LENGTH**
```bash
  SELECT tweet_id FROM Tweets
  WHERE CHAR_LENGTH(content) > 15; 
```

NOTE: **LENGTH** retunrs length in **bytes** & **CHAR_LENGTH** in **Number of Characters**.

# Basic Joins
## 6. Replace Employee ID With The Unique Identifier

Here we need to use the **JOIN** statement appropriately. Since we had all names in left and all unique_id in right..and we had to return all the names...we used **LEFT JOIN**.

```bash
  SELECT eu.unique_id, e.name
  FROM Employees e 
  LEFT JOIN EmployeeUNI eu
  ON e.id = eu.id;
```

NOTE: To use **RIGHT JOIN** we will have to simply select **EmployeeUNI** first and then **Employee**. Answer will be same.

## 7. Product Sales Analysis I

This question is same as **Question 6**.

```bash
  SELECT p.product_name, s.year, s.price
  FROM Sales s 
  LEFT JOIN Product p
  on s.product_id = p.product_id;
```

NOTE: To use **RIGHT JOIN** we will have to simply select __Product__ first and then **Sales**. Answer will be same.


## 8. Customer Who Visited but Did Not Make Any Transactions

In this questions we need to find number of customers who visited the mall but never made any transaction. Also find numbers of times they visited and never made any transaction.

- Here first **LEFT JOIN Visits** & **Transactions** and join them on a common paramenter that is **visit_id**.

- Now we get all the rows including those where transaction never happened. They will be **Null**.

- Now select all the rows where the **transaction_id** is **NULL**. 

- Use **COUNT** funtion to count the number of visits. **Count or Any Aggregate Funciton** is always follwed by **GROUP BY**.

- **GROUP BY** comes after **WHERE** clause.

```bash
  SELECT v.customer_id, 
  COUNT(v.customer_id) AS count_no_trans
  FROM Visits v
  LEFT JOIN Transactions t
  ON v.visit_id = t.visit_id
  WHERE t.transaction_id IS NULL
  GROUP BY v.customer_id;
```

NOTE: Breakdown the problem into smaller steps and then solve them. First step is to find out the Join and analyise the resutls of Join.

## 9. Rising Temperature (DATEDIFF)

This question already tells us that we need to **Self Join**. The major challenge here might be to join in such a way that the previous day temperature is joined for every row. 

- Every row must contain its data and also the temperature of **Previous Day**. 

- We Can use **INNER JOIN** here. Since the table is same, any join will work.

- Once this is done simply compare the 2 two columns. **Present Day Temp** and **Previous Day Temp** and return the **ID** if the temperature of a day is *greater* than the previous day.

```bash
  SELECT w1.id
  FROM Weather w1
  INNER JOIN Weather w2
  WHERE DATEDIFF(w1.recordDate,w2.recordDate) = 1
  AND w1.temperature > w2.temperature;
```

NOTE: **DATEDIFF** Function helps us find difference between the Dates. 

## 10. Average Time of Process per Machine


```bash
  SELECT w1.id
  FROM Weather w1
  INNER JOIN Weather w2
  WHERE DATEDIFF(w1.recordDate,w2.recordDate) = 1
  AND w1.temperature > w2.temperature;
```

NOTE: **DATEDIFF** Function helps us find difference between the Dates.



