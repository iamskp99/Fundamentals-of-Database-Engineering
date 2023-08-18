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











### Difference between primary and secondary key


