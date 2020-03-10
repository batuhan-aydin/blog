+++ 
date = "2020-03-10"
title = "Expanding PostgreSQL Knowledge"
slug = "expanding-postgresql-knowledge" 
tags= ["database","postgresql"]
+++

I already have enough SQL knowledge, it's really useful in both job and personal projects.
I am able to do CRUD operations, joins&relationships very easily. Recently, i was extending these
simple knowledge. I wanted to write what i've learnt, so later i can come back check again, in case i forget.

1- Views

Views are like, you write a query and call the whole query using only one name you've saved.

As we can see, we save the queary to myview, then we can use that name
just like a table name.

```sql
CREATE VIEW myview AS
    SELECT city, temp_lo, temp_hi, prcp, date, location
        FROM weather, cities
        WHERE city = name;

SELECT * FROM myview;
```

2- Data transactions

Data transactions mean, you have a bunch of operations, but you want all or none.
If one of the operations fail, none of them will be executed. In postgresql, you use
beign and commit to do that.

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100.00
    WHERE name = 'Alice';
-- etc etc
COMMIT;
```

If something goes wrong, we can just rollback instead of commit, so everything will be fine.
There is also save points.

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100.00
    WHERE name = 'Alice';
SAVEPOINT my_savepoint;
UPDATE accounts SET balance = balance + 100.00
    WHERE name = 'Bob';
-- oops ... forget that and use Wally's account
ROLLBACK TO my_savepoint;
UPDATE accounts SET balance = balance + 100.00
    WHERE name = 'Wally';
COMMIT;
```
It rollbacks to saved point, then sets Wally's account instead of Bob's.

3- Window Functions

Aggregate functions group the elements by columns. However, window functions are the opposite,
they group by the rows.

```sql
SELECT depname, empno, salary, avg(salary) OVER (PARTITION BY depname) FROM empsalary;
```
