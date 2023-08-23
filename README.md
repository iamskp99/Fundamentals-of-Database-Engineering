# PSQL

1) To list all the databases, use \l
2) For creating a database , use CREATE DATABASE "database_name";
3) To switch to another database, use \c database_name
4) To list all the database tables, use \dt
5) To describe the table use \d table_name

### Alter a table by adding a new column and create 1 Million fields
```
ALTER TABLE transactionspractice 
ADD COLUMN balance INT;
```
AND you can use this script

********************************************
-- Create a test table

```
CREATE TABLE test_data (
    id SERIAL PRIMARY KEY,
    value INTEGER
);

-- Generate a million rows with random integer values
INSERT INTO test_data (value)
SELECT floor(random() * 1000000) + 1
FROM generate_series(1, 1000000);

-- Check the number of rows
SELECT count(*) FROM test_data;

```
********************************************

### Update particular rows and their fields

```
UPDATE taskfailurereason
SET description = 'New description'
WHERE reason_id = 123;

```

### For Dropping a column

```
ALTER TABLE table_name
DROP COLUMN column_name;

```

### For making an index

```
CREATE INDEX index_name ON table_name (column_name);
```

### Some SQL Funcs and their uses

1. Getting second highest number

```
SELECT MAX(salary) AS SecondHighestSalary
FROM Employee WHERE salary NOT IN 
(SELECT MAX(salary) FROM Employee);
```

2. Getting the Nth highest number

```
LIMIT IS USED TO limit the number the number of rows.
OFFSET IS USED TO skip the number given to it

REATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
  SET N = N-1;
  RETURN (
      # Write your MySQL query statement below.
      SELECT DISTINCT(salary) FROM Employee ORDER BY salary DESC LIMIT 1 OFFSET N
  );
END

```

3.  Getting the rank based on a column

Here dense_rank, SUM, AVG all are examples of window functions and
OVER is used for them to specify the parameter on which these functions
will work.

```
SELECT score, dense_rank() OVER (ORDER BY score DESC) AS 'rank' FROM Scores;
```



### If you want how atomicity works

If you want to know how atomicity works then you can play with trasactions

```
begin transaction;
\\\ your query goes here 
commit;
```


### What are different isolation levels


![image](https://github.com/iamskp99/sql_practice/assets/42648568/d3a57111-8a41-4a08-84d8-2900484d6068)



### Column based databases (What are they and why they are used in OLAP databases)

Example of how they are stored

```
1:1001, 2:1002, 3:1003, 4:1004, 5:1005......, 100:1100,
Shashank:1001, Mayank:1002, Tushar:1003, Ritik:1004, Cool:1005......, Gaurav:1100,
..............
```

They are stored like column_value:row_id
Now, when we fetch try to fetch data for a single column , then only needed data will be fetched. Therefore less I/O
operations will be needed.

### Difference between primary and secondary key

All the keys in postgres are secondary key. Generally, the tables are index organized or they are called Index Organized table. In this case a primary key is used. For an index based on secondary key, we need to mantain an index. We can't use clustered based indexing.

### Why can't we use just redis instead of bloom filters ?

Let's consider a scenario where we want to see if a username exists in db or not then we can use redis to do direct lookup instead of using bloom filters

Using Redis for this purpose provides a precise way to verify whether a username exists or not. 

Bloom filters, on the other hand, might be used in situations where memory efficiency is more critical and where some level of approximation is acceptable. For example, in scenarios where you have a very large set of usernames and you want to quickly filter out obvious non-existent usernames before querying the database, a Bloom filter could be used. Keep in mind that using a Bloom filter in this case introduces the possibility of false positives, meaning the filter might incorrectly indicate that a username exists when it actually doesn't.




