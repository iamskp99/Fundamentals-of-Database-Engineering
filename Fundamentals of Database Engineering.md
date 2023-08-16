# PSQL

1) To list all the databases, use \l
2) For creating a database , use CREATE DATABASE "database_name";
3) To switch to another database, use \c database_name
4) To list all the database tables, use \dt
5) To describe the table use \d table_name

### Alter a table by adding a new column and create 1 Million fields

ALTER TABLE transactionspractice 
ADD COLUMN balance INT;

OR you can use this script

********************************************
-- Create a test table
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
********************************************

### How Postgres deals with ACID Properties and Eventual consistency

