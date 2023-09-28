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

Another Example

```
SELECT D.Name AS Department ,E.Name AS Employee ,E.Salary 
from 
	Employee E,
	Department D 
WHERE E.DepartmentId = D.id 
  AND (DepartmentId,Salary) in 
  (SELECT DepartmentId,max(Salary) FROM Employee GROUP BY DepartmentId);
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


### Vertical vs Horizontal Partitioning

Horizontal Partitioning : Suppose you have a table containing million rows so we can split that table into multiple table on the basis of id and we can save precious query time.
Vertical Partitioning : Large column (blob) that you can store in a slow access drive in its own table space.

### Horizontal Partitioning vs Sharding

HP splits big table into smaller tables in the same database , client is not aware (agnostic).
Sharding splits big table into multiple tables across multiple database servers. (This is used for distributed processing.) but the client is aware that it is hitting different servers.
This is a big problem.


### Pros and Cons of Partitioning

Pros

1. Improves query performance when accessing a single partition.
2. In sequential scan, it will come handy.
3. It is easier to attach partition.
4. Archive old data that are barely accessed into cheap storage.

Cons

1. Updates that move rows from a partition to another partition are slow or fail sometimes.
2. Inefficient queries could accidently scan all the partition resulting in slower scans.
3. Schema changes can be challenging.


### How does sharding deals with atomic transactions ?


Sharding is a database partitioning technique used to improve scalability and performance in distributed database systems. It involves dividing a large database into smaller, more manageable pieces called shards, and each shard is stored on a separate database server. While sharding can significantly improve performance and distribute the workload, it introduces challenges when dealing with atomic transactions, as maintaining the integrity of transactions across multiple shards can be complex.

Here's how sharding deals with atomic transactions:

1. **Local Transactions:** In a sharded database, each shard operates as an independent database system, capable of handling local transactions within its own data. These local transactions are atomic within the boundaries of a single shard, meaning they ensure data consistency and integrity within that shard.

2. **Distributed Transactions:** When a transaction involves data across multiple shards, it becomes a distributed transaction. Ensuring atomicity in distributed transactions is more challenging because you need to ensure that either all participating shards commit changes successfully, or none of them commit any changes at all (the "all-or-nothing" principle of atomicity).

3. **Two-Phase Commit (2PC):** One common approach to handling distributed transactions in sharded databases is to use a protocol called the Two-Phase Commit (2PC). In the first phase, the coordinator (often the application server or a dedicated transaction manager) sends a "prepare" message to all shards involved in the transaction. Each shard then checks if it can successfully commit the transaction. If all shards can commit, they reply with a "ready" message. In the second phase, the coordinator sends a "commit" or "abort" message to all shards based on the responses received in the first phase.

   - If all shards respond with "ready," the coordinator sends a "commit" message to all shards, and they proceed to commit the transaction.
   - If any shard responds with "abort" or if the coordinator times out waiting for responses, the coordinator sends an "abort" message to all shards, and they roll back the transaction.

4. **Challenges and Considerations:** While 2PC provides a way to achieve atomicity in distributed transactions, it has some drawbacks, such as performance overhead and the potential for blocking if a shard becomes unavailable. Alternative approaches like 3PC (Three-Phase Commit) and Paxos may be considered for more fault-tolerant and scalable solutions.

5. **Isolation and Consistency:** Sharded databases also need to consider transaction isolation levels (e.g., READ COMMITTED, SERIALIZABLE) to ensure consistency across shards. Implementing these isolation levels in a distributed environment requires careful design and coordination.

6. **Monitoring and Management:** Managing distributed transactions in a sharded database system requires robust monitoring and management tools to detect and handle issues like network failures, shard unavailability, and transaction timeouts.

In summary, sharding can be a powerful technique for improving database scalability, but it introduces complexity when dealing with atomic transactions. To ensure atomicity in distributed transactions, protocols like 2PC are often used, but they come with their own challenges and trade-offs. Database administrators and developers must carefully design and manage the distributed transaction process to maintain data integrity and consistency in a sharded environment.


