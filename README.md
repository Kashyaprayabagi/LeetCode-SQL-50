
# Leet Code - SQL 50 

Basics of SQL and solutions to all LeetCode SQL50 will be available in this file.




## 1. Recyclable and Low Fat Products


The Solution for this is to find all the products that have low fats and are recylable. We need to return their IDs.

```bash
  SELECT product_id FROM Products
  WHERE low_fats = 'Y' AND recyclable = 'Y';
```

**NOTE**: The AND operator is used here.

## 2. Find Customer Referee


The Solution for this is to find the names of Customer where referee_id is not equal to 2. 

```bash
  SELECT name FROM Customer
  WHERE referee_id != 2 or referee_id IS NULL;    
```

**NOTE**: SQL **do not** return values with **NULL** operator by default. Hence we need to specify it seperately.

## 3. Big Countries


The Solution for this is to find the names, population and area of countries that either has area >= 3000000 or a population >= 25000000.
```bash
  SELECT name, population, area
  FROM World WHERE 
  area >= 3000000 OR population >= 25000000;    
```

**NOTE**: **OR** operator is used to do the same.

## 4. Article Views I


The Solution for this is to find the tuples that have same author_id and viewer_id. However their might be many authors who read their own articles atleast once..so we are asked to return the unique values of the author_id.
```bash
  SELECT DISTINCT author_id AS id 
  FROM Views WHERE author_id = viewer_id
  ORDER BY id;   
```

**NOTE**: **DISTINCT** keyword will return only unique values. **Specify the coloumn_name after ORDER BY clause.** **ASC** or **DESC** can be written later.

## 5. Invalid Tweets (CHAR_LENGTH)


The challenge in this question is to find the lenght of the string. There are 2 methods to do it **LENGTH** and **CHAR_LENGTH**
```bash
  SELECT tweet_id FROM Tweets
  WHERE CHAR_LENGTH(content) > 15; 
```

**NOTE**: **LENGTH** retunrs length in **bytes** & **CHAR_LENGTH** in **Number of Characters**.

# Basic Joins
## 6. Replace Employee ID With The Unique Identifier

Here we need to use the **JOIN** statement appropriately. Since we had all names in left and all unique_id in right..and we had to return all the names...we used **LEFT JOIN**.

```bash
  SELECT eu.unique_id, e.name
  FROM Employees e 
  LEFT JOIN EmployeeUNI eu
  ON e.id = eu.id;
```

**NOTE**: To use **RIGHT JOIN** we will have to simply select **EmployeeUNI** first and then **Employee**. Answer will be same.

## 7. Product Sales Analysis I

This question is same as **Question 6**.

```bash
  SELECT p.product_name, s.year, s.price
  FROM Sales s 
  LEFT JOIN Product p
  on s.product_id = p.product_id;
```

**NOTE**: To use **RIGHT JOIN** we will have to simply select __Product__ first and then **Sales**. Answer will be same.


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

**NOTE**: Breakdown the problem into smaller steps and then solve them. First step is to find out the Join and analyise the resutls of Join.

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

**NOTE**: **DATEDIFF** Function helps us find difference between the Dates. 

## 10. Average Time of Process per Machine

Here we need to find the average time taken by every machine to complete all the assigned processes. To do this we need to do the following:

- use **INNER JOIN** to join the Activity table with *itself*.

- The **condition** on Inner Join should be the equivalance of both **machine_id** and **process_id**. The activiity_type shoudl be *different*.

- Now use the **AVG** function to find the average of *sum of time taken by all the processes by a machine*

- Round the average using the **ROUND** function and mention the number of decimal places.

- Finally use the **GROUP BY** funtion to group machines with same *machine_id*.

```bash
  SELECT A1.machine_id, 
  ROUND(AVG(A2.timestamp - A1.timestamp),3) 
  AS processing_time
  FROM  Activity A1
  INNER JOIN Activity A2
  ON A1.machine_id = A2.machine_id 
  AND A1.process_id = A2.process_id 
  AND A1.activity_type = "start" 
  AND A2.activity_type = "end"
  GROUP BY machine_id;

```

**NOTE**: Aggregate funcitons are to be used logically. Every solution need not have subqueires.


## 11. Employee Bonus

This question is same as **Question 6 & 7**.

```bash
  SELECT e.name, b.bonus
  FROM Employee e
  LEFT JOIN Bonus b
  ON e.empId = b.empId
  WHERE b.bonus < 1000 OR b.bonus IS NULL;
```

**NOTE**: To use **RIGHT JOIN** we will have to simply select **Bonus** first and then **Employee**. Answer will be same. We can also use **HAVING** instead of **WHERE**.

## 12. Students and Examinations

This question has confused me 😭.

```bash
  SELECT s.student_id, s.student_name, sub.subject_name, COUNT(e.subject_name) AS attended_exams
  FROM Students s
  CROSS JOIN Subjects sub
  LEFT JOIN Examinations e
  ON s.student_id = e.student_id AND sub.subject_name = e.subject_name
  GROUP BY s.student_id, s.student_name, sub.subject_name
  ORDER BY s.student_id, sub.subject_name;
```

**NOTE**: ***Look at this question again***.

## 13. Managers with at Least 5 Direct Reports

Here, We need to **Self Join** the Employee table on the conditiion of ***equivalencce of Manager ID and Employee ID***. use **GROUP BY** to group the Employee ID and count it using the **HAVING** clause. 

```bash
  SELECT e2.name
  FROM Employee e1
  JOIN Employee e2
  ON e1.managerId = e2.id
  GROUP BY e2.id
  HAVING COUNT(e2.id) >= 5;
```

**NOTE**: **HAVING** is always used ***after*** the **Group By** clause. It can be used along side any aggregate functions.

## 14. Confirmation Rate (IFNULL)

Here, We need to **LEFT JOIN** the Signups table with Confirmation table on the condition of ***equivalance of user IDs*** and then we need to **SUM** all the *confirm actions* and *divide* by the *total no of actions*. 

```bash
  SELECT s.user_id, IFNULL(
    ROUND((SUM(c.action = "confirmed")/COUNT(*)),2),0.00) AS confirmation_rate
  FROM Signups s
  LEFT JOIN Confirmations c
  ON s.user_id = c.user_id
  GROUP BY s.user_id;
```

**NOTE**:**ROUND** clause will help us round off the bits to to *n decimal places*. **IFNULL** clause will check if the values are null and assign them with a default value.


## 15. Not Boring Movies

Here, We need to simply use the **%** operator to filter the *odd* IDs.

```bash
  SELECT *
  FROM Cinema c
  WHERE (c.id % 2) != 0 
  AND description != "boring"
  ORDER BY c.id DESC;
```

## 16. Average Selling Price

Here, We need to find the average price of a product, given that its price varies from one date to another. 

- The average price can be found by multipling the units bought with unit cost in the given date and diving it by the total number of units.

- Since it is said that a product *might not be sold* at times, we need to use **LEFT JOIN** to join the *Prices* & *UnitsSold* table.

- Round the **SUM** of the *product of prices and units/sum of units* to 2 decimals and check for the **IFNULL** Condition.

```bash
  SELECT p.product_id, IFNULL(ROUND(SUM(p.price * u.units)/SUM(u.units),2),0) AS average_price
  FROM Prices p
  LEFT JOIN UnitsSold u
  ON p.product_id = u.product_id
  AND u.purchase_date >= p.start_date AND u.purchase_date <= p.end_date
  GROUP BY p.product_id;
```

**NOTE**: Break the problem into smaller parts and remember *Date* can be compared using the ***Comparison*** operators.


## 17. Project Employees I

Here, We need to find the average of experience of all the employees working  on a particlar porject. Simply find the *sum of all the experience years* and divide by *no of employees in a project*. 

- Use **LEFT JOIN** to join the *Project* and *Employee* table on the equivalance of *employee ID*.

```bash
  SELECT p.project_id, ROUND(SUM(e.experience_years)/COUNT
  (e.employee_id),2) AS average_years
  FROM Project p
  LEFT JOIN Employee e
  on p.employee_id = e.employee_id
  GROUP BY project_id;
```

**NOTE**: Break the problem into smaller parts and solve it.

## 18. Percentage of Users Attended a Contest

Here, We need to find out the percentage of users that registered for every event.

- Simply use the **LEFT JOIN** on the equivalanace of the **user ID**.

- count the *Number of users* for each even and divide by *total no of users*.

- Use **GROUP BY** and **ORDER BY** in order to get the required ordering.

```bash
  SELECT r.contest_id, ROUND((COUNT(r.user_id) * 100/
  (SELECT COUNT(*) user_id FROM Users)),2) as percentage
  FROM Register r
  LEFT JOIN Users u
  on r.user_id = u.user_id
  GROUP BY r.contest_id
  ORDER BY percentage DESC, r.contest_id ASC;
```

**NOTE**: Remember You can always use a **SELECT** clause *inside* another SELECT Clause.

## 19. Queries Quality and Percentage

Here, We need to find 2 things. One is Quality and the other is Poor Query Percentage. Now to do this:

- First for *Quality*, You can either **SUM(rating/position)/Count(rating)** or directly use **AVG** clause as below.

- For the second column, we used an ***IF*** clause to find if the rating is *below 3* and then let it *return 1* if true and took an **AVG** of it.

```bash
  SELECT query_name, ROUND(AVG(rating/position),2) AS 
  quality, ROUND(AVG(IF(rating < 3,1 ,0))*100,2) AS 
  poor_query_percentage
  FROM Queries 
  GROUP BY query_name;
```

**NOTE**: There is an **IF** Function in MYSQL and use it liberaly.

## 20. Monthly Transactions I (Too much of learning)

Here, We need to find *No of transactions*, *No of approved transaction*, *Sum of all transactions made in a month and a country*, *Sum of all approved transactions made in a month and a country*.

- use the **DATE_FORMAT** function to only get month and year. (Using captial letter in month will return the ***NAME*** of the month),

- **COUNT** all the transactions, **SUM** all the transacations.

- To sum all the *approved transactions* use the **IF** function to see if the state is approved or not. If approved **SUM** it else *0*.

```bash
  SELECT
    DATE_FORMAT(trans_date, '%Y-%m') AS month,
    country,
    COUNT(id) AS trans_count,
    SUM(state = "approved") AS approved_count,
    SUM(amount) AS trans_total_amount,
    SUM(IF(state = "approved",amount,0)) AS 
    approved_total_amount
  FROM Transactions
  GROUP BY month, country
```

**NOTE**: When using the **COUNT** Function, it counts all the ***NON NULL*** values hence it will count *both approved and not approved*. Hence use the **SUM** function to get it right.













