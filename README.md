
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








