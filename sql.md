

# SQL & PostgreSQL

### Setup

let's deploy PostgreSQL and pgadmin by docker compose

`docker-compose.yml`

```yaml
version: '3.5'

services:
  postgres:
    container_name: postgres_container
    image: postgres
    environment:
      POSTGRES_USER: ${POSTGRES_USER:-postgres}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-changeme}
      PGDATA: /data/postgres
    volumes:
       - postgres:/data/postgres
    ports:
      - "5432:5432"
    networks:
      - postgres
    restart: unless-stopped
  
  pgadmin:
    container_name: pgadmin_container
    image: dpage/pgadmin4
    environment:
      PGADMIN_DEFAULT_EMAIL: ${PGADMIN_DEFAULT_EMAIL:-pgadmin4@pgadmin.org}
      PGADMIN_DEFAULT_PASSWORD: ${PGADMIN_DEFAULT_PASSWORD:-admin}
    volumes:
       - pgadmin:/root/.pgadmin
    ports:
      - "${PGADMIN_PORT:-5050}:80"
    networks:
      - postgres
    restart: unless-stopped

networks:
  postgres:
    driver: bridge

volumes:
    postgres:
    pgadmin:

```

run `docker-compose up -d`





----

![image-20200421022943555](/home/aviad/.config/Typora/typora-user-images/image-20200421022943555.png)





### Select

to query from database

```sql
SELECT * FROM table_1

SELECT col1,col2 FROM table_1
```

after `SELECT` we decide which column and after the `FROM` we decide from which table

`*` will represent all columns from table.

this is not good practice to use `*` , this may slow down the query.

best practice is to take the required columns.



one of the convention of readability of SQL is to write the commands of SQL with big letters.



### Distinct & Count

get unique value

```sql
SELECT DISTINCT(column) FROM table

SELECT COUNT(*) FROM customer
```



count

```sql
SELECT COUNT(column) FROM table
```



count distinct

```sql
SELECT COUNT(column) FROM table

SELECT COUNT(DISTINCT first_name) FROM customer
-- WE CANNOT USE HERE * as columne
```



### Where

query what we want by `WHERE`

where will be after `FROM` 

Operators for `WHERE` :

- `=`
- `>`
- `<`
- `>=`
- `<=`
- `!=` `<>`

Logical Operators to combine couple Opeartors :

- `AND`
- `OR`
- `NOT`

```sql
SELECT name,choice FROM persons WHERE name='David' AND name='Aviad'; 
SELECT * FROM film WHERE rental_rate > 4 AND replacement_cost >= 19.99; 
```



### Order By

maybe the result can get in different order

when the order is important we can sort by `ORDER BY` in `ASC / DESC` order

`ORDER BY` will be in the end of the query

if you don't specify order , default is `ASC`

you can sort by couple of columns

for example sort by names then by salary.

```sql
SELECT * FROM customer ORDER BY first_name ASC
SELECT * FROM customer ORDER BY first_name DESC


SELECT store_id,first_name,last_name FROM customer ORDER BY first_name,first_name DESC
SELECT store_id,first_name,last_name FROM customer ORDER BY first_name DESC,first_name ASC
```

we can sort by field that we not select.





### Limit 

if you want just couple of results you can use the limit

```sql
SELECT * FROM payment WHERE amount != 0.00 ORDER BY payment_date DESC LIMIT 5;
```



### Between

we using it in `WHERE` 

we can combine between with `NOT` 

`BETWEEN` IS `X>5 AND X<10`

`NOT BETWEEN` IS `X<5 AND X>10`



can also be used with dates

`WHERE data BETWEEN '2007-01-01' AND '2007-02-01'`



```sql
SELECT COUNT(*) FROM payment WHERE amount NOT BETWEEN 8 AND 9;
-- same as
SELECT COUNT(*) FROM payment WHERE amount >= 8 AND amount <= 9;
```



### In

instead of using a lots of `OR` operator

```sql
SELECT color FROM dall WHERE color IN('red','blue')
-- same as
SELECT color FROM dall WHERE color='red' OR color='blue'
```

we can either use `NOT IN` 



### Like & ILIKE

(similar to regex but with wild-cards)

check if string behavior in specific way for example : starts with letter f and ends with z , have 3 digits , etc..



`%` - sequence of character , this sequence can be blank

`_` - single character



`LIKE` is case sensitive

`ILIKE` is not case sensitive

```sql
WHERE name LIKE 'A%' -- starts with A
WHERE name LIKE '%a' -- contains lower case a 

WHERE value ILIKE 'VERSION#__' -- could be VERSION#12

```



PostgreSQL supports also the regex , this is different syntax from `LIKE` >> explore



example for queries :

```sql
SELECT COUNT(*) FROM payment WHERE amount>5.00 -- 3618
SELECT COUNT(DISTINCT(first_name)) FROM actor WHERE first_name LIKE 'P%' -- 2
SELECT COUNT(DISTINCT(district)) FROM address -- 378
SELECT DISTINCT(district) FROM address -- 378
SELECT COUNT(DISTINCT(title)) FROM film WHERE replacement_cost BETWEEN 5 AND 15 AND rating='R' -- 52
SELECT COUNT(DISTINCT(title)) FROM film WHERE title LIKE '%Truman%' --5
```





## Aggregations

get an output for multiple input

we are going to show you :

- `AVG()` - return average in a floating point value , you can use `ROUND()` to specify precision after the decimal.
- `COUNT()` - count number of rows.
- `MAX()` - get max for column.
- `MIN()` - get min for column.
- `SUM()` - get sum for column.



this happen with the `SELECT` or `HAVE`

```sql
SELECT MIN(replacement_cost) FROM film -- get min replacement_cost OUTPUT : min 9.99
SELECT MAX(replacement_cost) FROM film -- get max replacement_cost : OUTPUT max : 29.99

-- we will get only the value for the required column MIN/MAX
-- we can run MIN , MAX BOTH , or multiple min/max, or either with another aggergation 	      funciton

SELECT MIN(replacement_cost),MAX(replacement_cost) FROM film 
-- OUTPUT : min : 10.99 ,     max:22.99

SELECT AVG(replacement_cost) FROM film -- OUTPUT :  avg : 19.9840000000000000
SELECT ROUND(AVG(replacement_cost),2) FROM film -- OUTPUT :  round : 19.98
SELECT ROUND(AVG(replacement_cost),3) FROM film -- OUTPUT :  round : 19.984

SELECT SUM(replacement_cost) FROM film; -- OUTPUT sum : 19984.00


SELECT AVG(replacement_cost),SUM(replacement_cost), MAX(replacement_cost),MIN(replacement_cost),COUNT(replacement_cost) FROM film;
--OUTPUT : 	    avg: 19.9840000000000000 sum: 19984.00 min 9.99 max : 29.99 count : 1000
```



### GROUP BY

group allow you do this things

![image-20200421185806944](/home/aviad/.config/Typora/typora-user-images/image-20200421185806944.png)





```sql
SELECT catagory_col,AGG(data_col) FROM table GROUP BY catagory_col
```

`AGG` some aggregation operation like (sum,avg,count)

we can add where

```sql
SELECT catagory_col,AGG(data_col) 
FROM table
WHERE catagory_col!='A'
GROUP BY catagory_col
```



we have to take what we've selected to the `GROUP BY`

when we want to sort we have to put the `AGG(data_col)`

```sql
SELECT catagory_col,AGG(data_col) 
FROM table
WHERE catagory_col!='A'
GROUP BY catagory_col
ORDER BY AGG(data_col) 
```



what customer_id waist most money

```sql
SELECT customer_id,SUM(amount) FROM payment
GROUP BY customer_id
ORDER BY SUM(amount) DESC
```



how many transaction for each customer

```sql
SELECT customer_id,COUNT(amount) FROM payment
GROUP BY customer_id
ORDER BY COUNT(amount) DESC
```



how much many each customer spend for each stuff

```sql
SELECT customer_id,staff_id, COUNT(amount) FROM payment
GROUP BY staff_id,customer_id
ORDER BY customer_id
```



 sum of amount each day , when we want to group by date and the date is time-stamp we may use `DATE()` 

``` sql
SELECT DATE(payment_date),SUM(amount) FROM payment
GROUP BY DATE(payment_date)
ORDER BY SUM(amount) DESC
```



top 5 customers

```sql
SELECT customer_id,SUM(amount) FROM payment
GROUP BY customer_id 
ORDER BY SUM(amount) DESC
LIMIT 5
```



### Having

filter after aggregation operation

to filter before we you `WHERE`

to filter after we using `HAVING`

```sql
SELECT company,sum(sales)
FROM finance_table
GROUP BY company
HAVING SUM(sales) > 1000
```



```sql
SELECT customer_id,SUM(amount) FROM payment
WHERE customer_id NOT IN(184,87,477)
GROUP BY customer_id
HAVING SUM(amount) > 100
```



customers with more than 40 transcations

```sql
SELECT customer_id,COUNT(amount) FROM payment
GROUP BY customer_id
HAVING COUNT(amount) >= 40
ORDER BY COUNT(*) DESC
```



get customers that pay more than 100 dollar from staff2

```sql
SELECT customer_id,staff_id,SUM(amount)  FROM payment
GROUP BY customer_id,staff_id
HAVING staff_id=2 AND SUM(amount) > 100
ORDER BY SUM(amount) DESC
```





#### Questions

1. Return the customer IDs of customers who have spent at least $110 with the staff member who has an ID of 2.

```sql
SELECT customer_id,staff_id,SUM(amount)  FROM payment
GROUP BY customer_id,staff_id
HAVING staff_id=2 AND SUM(amount) >= 110
ORDER BY SUM(amount) DESC
```



2. How many films begin with the letter J?

```sql
SELECT COUNT(*) FROM film 
WHERE title LIKE 'J%'
```



3. What customer has the highest customer ID number whose name starts **with** an 'E' **and** has an address ID lower than 500?

```sql
SELECT first_name,last_name FROM customer 
WHERE address_id < 500 AND first_name LIKE 'E%'
ORDER BY customer_id DESC
LIMIT 1
```





## Joins



#### AS

as allow you create alias 

```sql
SELECT column AS new_name FROM table
```

this gets executed at the very end of a query then we can't use the alias inside `WHERE` or `HAVING` operations.

usually we use the alias in the `SELECT`





### Inner Join



records that exists in **both** tables

```sql
SELECT * FROM tableA INNER JOIN tableB  ON tableA.col_match = tableB.col_match

-- same as

SELECT * FROM tableB INNER JOIN tableA  ON tableB.col_match = tableA.col_match

```



if we want select fields we can just add it to the select

```sql
SELECT log_id, tableA.col_match FROM tableA INNER JOIN tableB  ON tableA.col_match = tableB.col_match

```

if we have the same name of fields in two tables

we need to put the table name before the column name

```sql
SELECT tableA.log_id tableA.col_match FROM tableA INNER JOIN tableB  ON tableA.col_match = tableB.col_match

```



real SQL query

```sql
SELECT * FROM payment INNER JOIN customer ON payment.customer_id = customer.customer_id
```

we can select field of customer

```sql
SELECT payment_id,payment.customer_id,customer.first_name 
FROM payment INNER JOIN customer ON payment.customer_id = customer.customer_id
```



![image-20200421234410796](/home/aviad/.config/Typora/typora-user-images/image-20200421234410796.png)





### Full outer join

```sql
SELECT * FROM Registrations FULL OUTER JOIN Logins ON Registrations.name = Logins.name
```



will take all the data from both tables.

columns with no fields display null



![image-20200422015927843](/home/aviad/.config/Typora/typora-user-images/image-20200422015927843.png)



![image-20200422020030192](/home/aviad/.config/Typora/typora-user-images/image-20200422020030192.png)





### Left Outer Join

### ![image-20200422020416719](/home/aviad/.config/Typora/typora-user-images/image-20200422020416719.png)





###  Cross Join

![image-20200421234750675](/home/aviad/.config/Typora/typora-user-images/image-20200421234750675.png)





### Right Join

all left joins similar to right join but in right join the table is switched.

instead of using right join we can replace the place of the tables in the query.

example for full outer right join



![image-20200422022923208](/home/aviad/.config/Typora/typora-user-images/image-20200422022923208.png)



### Unions

imagine that we have 2 tables

we don't want to join them 

we just want to display the ouput of query this to tables

for this case we can use UNION

```sql
SELECT * FROM Sales2021_Q1 
UNION
SELECT * FROM Sales2021_Q2
```



before

![image-20200422023521323](/home/aviad/.config/Typora/typora-user-images/image-20200422023521323.png)

after



![image-20200422023551156](/home/aviad/.config/Typora/typora-user-images/image-20200422023551156.png)









### examples

get all customers from California

```sql
SELECT email,address.district FROM customer INNER JOIN address 
ON customer.address_id = address.address_id
WHERE district ='California'
```



get all movies that Nick Wattenberg act.

we have here multiple joins.

```sql
SELECT title,first_name,last_name FROM  film_actor
INNER JOIN actor ON
film_actor.actor_id = actor.actor_id
INNER JOIN film ON
film_actor.film_id = film.film_id
WHERE actor.first_name = 'Nick' And actor.last_name = 'Wahlberg'

```



## Timestamps and Extract

we have different ways to represent time

`TIME` - contain hour min second

`DATE` - mount day year

`TIMESTAMP` = `TIME` + `DATE`

`TIMESTAMPZ` = `TIMESTAMP` + `ZONE`



functions and operations related to specific data types :

- `TIMEZONE`
- `NOW`

- `TIMEOFDAY`
- `CURRENT_TIME`
- `CURRENT_DATE`





to check you timezone run

`SHOW TIMEZONE`

```SELECT NOW() ``` get current time zone `2020-04-22 10:31:49.193326+00`

`SELECT TIMEOFDAY()` - same info as string `Wed Apr 22 10:32:51.496070 2020 UTC` 

`SELECT CURRENT_TIME` - `10:33:41.547863+00:00`

`SELECT CURRENT_DATE` - `2020-04-22`



we can extract from date the year , month ,day ,week, quarter

if you only interested in year

```EXTRACT(YEAR FROM date_col)```



`AGE(date_col)` calculate the age of give timestamp



convert date types

```TO_CHAR(date_col,'mm-dd-yyyy')```



examples :

get all months payments

```sql
SELECT DISTINCT(TO_CHAR(payment_date,'MONTH')) FROM payment 
```



get all payments from monday

```plsql
SELECT COUNT(*) FROM payment 
WHERE TO_CHAR(payment_date,'Day') LIKE 'Monday%'
```





### Operations 

we can preform math operations

```plsql
SELECT ROUND(rental_rate/replacement_cost,2)*100 FROM film
SELECT 0.1 * replacement_cost FROM film
```

more details [here](https://www.postgresql.org/docs/9.5/functions-math.html) 



string operations

```plsql
SELECT LENGTH(first_name) FROM customer -- get length of string
SELECT first_name || last_name FROM customer -- connect to string together 
SELECT first_name || last_name || '--' || email FROM customer -- connect to string together 

SELECT UPPER(first_name) FROM LOWER(customer) -- upper case letters and lower case
SELECT LEFT(first_name,2) FROM customer -- get only first 2 letters
```

more details [here](https://www.postgresql.org/docs/9.1/functions-string.html)



## SubQuery

sub query allow you make a query on your query.



for example :

we have table of students and their grade 

we want to get all students that there grades is bigger than the average grade

in this case we'll need to use sub query.

```sql
SELECT student,grade FROM test_scores
WHERE grade > (SELECT AVG(grade) FROM test_scores)
```



count how many movies have rental rate that bigger avg rate

```plsql
SELECT COUNT(*) FROM film 
WHERE rental_rate > (SELECT AVG(rental_rate) FROM film)
```



if the subquery return more that one value we can use the in operator

```sql
SELECT student,grade FROM test_scores
WHERE student IN (
	SELECT student FROM honor_roll_table
)

-- will look like
-- SELECT student,grade FROM test_scores WHERE student IN (('Zach','Chris','Karissa'))
```



`EXISTS` 

used to test for existence of rows in a sub-query 



```sqlite
SELECT first_name,last_name FROM customer AS c
WHERE EXISTS(SELECT * FROM payment as p 
     		WHERE p.customer_id = c.customer_id AND amount > 11)
-- SAME AS 

SELECT first_name,last_name FROM customer 
INNER JOIN payment ON customer.customer_id = payment.customer_id
WHERE amount > 11
```



## Self Join

A self join is a query in which a table is joined to itself.

Self joins are useful for comparing values in a column of rows whiting the same table.

```sql
SELECT tableA.col, tableB.col
FROM table AS tableA
JOIN table AS tableB ON
tableA.some_col = tableB.other_col
```

notice that we join the same table !! just named the table in different name.



for example

![image-20200423160744576](/home/aviad/.config/Typora/typora-user-images/image-20200423160744576.png)

in this table we have employees

each employee has `emp_id` and `report_id` which represent to who send report.

if we want to represent this as name of employee , name of employee to report

we need to make inner join.

![image-20200423160901975](/home/aviad/.config/Typora/typora-user-images/image-20200423160901975.png)

the query will be

```sql
SELECT emp.name,report.name AS rep
FROM employee AS emp
JOIN employee AS report 
ON emp.emp_id = report.report_id
```



get movies with the same length 

```sql
SELECT tableA.title AS movieA ,tableB.title AS movieB , tableA.length
FROM film AS tableA
JOIN film AS tableB 
ON tableA.length = tableB.length AND tableA.film_id != tableB.film_id
ORDER BY tableA.length DESC
```





Answers

1. ```sql
   SELECT * FROM cd.facilities
   ```

2. ```sql
   SELECT name,membercost FROM cd.facilities
   ```

3. ```sql
   SELECT * FROM cd.facilities WHERE membercost > 0
   ```

4. ```sql
   SELECT facid,name,membercost,monthlymaintenance FROM cd.facilities
   WHERE membercost < monthlymaintenance / 50 AND membercost != 0
   ```

5. ```sql
   SELECT * FROM cd.facilities WHERE name LIKE '%Tennis%'
   ```

6. ```sql
   SELECT * FROM cd.facilities WHERE facilities.facid = 1 
   UNION
   SELECT * FROM cd.facilities WHERE facilities.facid = 5
   ```

7. ```sql
   SELECT memid,surname,firstname,joindate 
   FROM cd.members 
   WHERE joindate > '2012-09-01'
   ```

8. ```sql
   SELECT DISTINCT(surname),joindate FROM cd.members 
   LIMIT 10 
   ```

9. ```sql
   SELECT joindate FROM cd.members 
   ORDER BY joindate DESC
   LIMIT 1
   ```

10. ```sql
    SELECT COUNT(*) FROM cd.facilities WHERE guestcost >= 10;
    ```

11. ```sql
    SELECT facid,SUM(slots) AS slots FROM cd.bookings
    WHERE starttime > '2012-09-01' AND starttime < '2012-10-01'
    GROUP BY facid
    ORDER BY SUM(slots) ASC
    ```

12. ```sql
    SELECT facid,SUM(slots) AS slots FROM cd.bookings
    GROUP BY facid
    HAVING SUM(slots)>1000
    ORDER BY SUM(slots) DESC
    ```

13. ```sql
    SELECT * FROM cd.bookings
    WHERE cd.bookings.facid IN 
    (SELECT facid FROM cd.facilities WHERE name LIKE 'Tennis Court %')
    AND starttime BETWEEN '2012-09-21' AND '2012-09-22'
    
    -- to get the name
    
    SELECT cd.bookings.starttime,cd.facilities.name AS start  FROM cd.facilities
    INNER JOIN cd.bookings ON 
    cd.facilities.facid = cd.bookings.facid
    WHERE starttime BETWEEN '2012-09-21' AND '2012-09-22' AND cd.facilities.name LIKE 'Tennis Court %'
    ```

14. 

    ```sql
    SELECT * FROM cd.bookings 
    INNER JOIN cd.members
    ON cd.bookings.memId = cd.members.memId
    WHERE firstname='David' AND surname='Farrell'
    ```

    



# Creating Tables



#### Data types :

- Boolean
  - true
  - false
- Character
  - char
  - varchar
  - text
- Numeric
  - integer 
  - float
- Temporal
  - date
  - time
  - timestamp
  - interval
- UUID 
- Array (of type)
- JSON
- Hstore key-value pair
- Special such as : network address,geometric data , etc..



when we create a table we specify to each column which type.

we have [here](https://www.postgresql.org/docs/current/datatype.html) details about all data types.



types of numeric

![image-20200423194425907](/home/aviad/.config/Typora/typora-user-images/image-20200423194425907.png)



#### Primary and Foreign Keys

Primary Key (pk) used to identify a row.

Foreign Key (fk) identify a row in another table.



table that contains the foreign key is called referencing table or child table.

table to which the foreign key references is called referenced table or parent table.



for example we have customer table

and we have payments



payment refer to 3 another tables.

![image-20200423195341744](/home/aviad/.config/Typora/typora-user-images/image-20200423195341744.png)





#### Constraints

constrains are rules enforced on data columns on table.

These are used to prevent invalid data from being entered into the database.

This ensures the accuracy and reliability of the data in the database.



can be divided into 2 categories :

- column constrains - check data in column
- table constrains - applied to the entire table rather than to an individual column.



most common column constrains :

- `NOT NULL` - column cannot have null value
- `UNIQUE` - Ensure that all values in a column are different
- `Primary Key` 
- `Foreign Key`
- `CHECK` - all values in a column satisfy certain conditions.
- `EXCLUSION` - ?



most common table constrains :

- `CHECK` - check a condition when inserting or updating data.
- `REFERENCES` - the value stored in the column that must exist in a column in another table.
- `UNIQUE` - filed that need to be unique in multiple columns.
- `PRIMARY KEY` - define pk that exists in multiple columns.



#### CREATE Table



```sql
CREATE TABLE table_name (
	column_name TYPE column_constraint,
    column_name TYPE column_constraint,
    table_constraint table_constraint 
) INHERITS exisiting_table_name;
```







```plsql
CREATE TABLE players(
	player_id SERIAL PRIMARY KEY,
    age SMALLINT NOT NULL
)
```

`SERIAL` - create sequence of integers , this is perfect for a pk , 
because it logs unique integer entries for you and automatically upon insertion.



```sql
CREATE TABLE account(
	user_id SERIAL PRIMARY KEY,
	username VARCHAR(50) UNIQUE NOT NULL,
	password VARCHAR(50) NOT NULL,
	email VARCHAR(250) UNIQUE NOT NULL,
	create_on TIMESTAMP NOT NULL,
	last_login TIMESTAMP
)
```

```sql
CREATE TABLE job(
	job_id SERIAL PRIMARY KEY,
	job_name VARCHAR(200) UNIQUE NOT NULL
)
```

```sql
CREATE TABLE account_job(
	user_id INTEGER REFERENCES account(user_id),
	job_id INTEGER REFERENCES job(job_id),
	hire_date TIMESTAMP
)
```



#### INSERT TABLE

```sql
-- insert multiple rows
INSERT INTO table(column1,column2,...)
VALUES
(value1,valu2,...),
(value1,valu2,...),
(value1,valu2,...),
(value1,valu2,...),
(value1,valu2,...);

-- insert from another table
INSERT INTO table(column1,column2,...)
SELECT column1,column2,..
FROM another_table
WHERE condition;
```



```sql
INSERT INTO account(username,password,email,create_on) 
VALUES
('Avi','hashedPassword','avi@email.com',CURRENT_TIMESTAMP)
```

```sql
INSERT INTO job(job_name) 
VALUES
('SE'),
('Teacher');
```

```plsql
INSERT INTO account_job(user_id,job_id,hire_date)
VALUES 
(1,1,CURRENT_TIMESTAMP)
```

if we will give to `account_job` fk that doesn't exists this will raise an error.



#### UPDATE table

```sql
UPDATE table_name
SET column1 = value1,
	column2 = value2
WHERE 
	condition;
```



update all accounts that didn't log in.

```sql
UPDATE account
SET last_login = CURRENT_TIMESTAMP
WHERE last_login IS NULL
```

we can not use `WHERE` then this will update all columns in table.



get value to update from another table

```sql
UPDATE account_job
SET hire_date = account.created_on
FROM account
WHERE account_job.user_id = account.user_id
```



return column after update

 ```plsql
UPDATE account
SET last_login = CURRENT_TIMESTAMP
RETURNING email,created_on,last_login
 ```



#### DELETE 

if we want to delete rows

```sql
DELETE FROM table_name
WHERE condition;
```

```plsql
DELETE FROM job
WHERE job_name = 'Cowboy'
RETURNING job_id,job_name
```



#### ALTER

change existing table structure

adding ,dropping , renaming columns 

changing a column data type

set default values for column

add check constraints 

rename table

```sql
ALTER TABLE table_name action
```



```plsql
--add coll
ALTER TABLE table_name
ADD COLUMN new_col TYPE_COL

--drop col
ALTER TABLE table_name
DROP COLUMN new_col TYPE_COL

--alter constraints
ALTER TABLE table_name
SET DEFAULT value

ALTER TABLE table_name
SET NOT NULL value

ALTER TABLE table_name
DROP NOT NULL value

ALTER TABLE table_name
SET CONSTRAINT constraint_name
```



real examples :

```plsql
CREATE TABLE information(
	info_id SERIAL PRIMARY KEY,
    title VARCHAR(500) NOT NULL,
    person VARCHAR(500) NOT NULL UNIQUE
)

-- rename table name
ALTER TABLE information
RENAME TO new_info

-- rename column
ALTER TABLE new_ifo
RENAME COLUMN person TO people


INSERT INTO new_info(title)
VALUES ('some new title')
-- this will not wil inserted cause we didn't put person

ALTER TABLE new_info
ALTER COLUMN people DROP NOT NULL

INSERT INTO new_info(title)
VALUES ('some new title')
-- now this will work


```





#### DROP 

allow remove column and all his indexes and constraints involving to the column 

it will not remove columns used in views,triggers , or stored procedures without the additional CASCADE clause.



```plsql
ALTER TABLE table_name 
DROP COLUMN col_name

ALTER TABLE table_name 
DROP COLUMN col_name CASCADE -- cascade remove all dependencies(fk,etc..)
```



#### CHECK

preform check before inserting

```plsql
CREATE TABLE employees(
	emp_id SERIAL PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    birthdate DATE CHECK(birthdate > '1900-01-01'),
    hire_date DATE CHECK(hire_date > birthdate)
	salary INTEGER CHECK(salary > 0)
)
```



# Conditional Expressions and Procedures



#### CASE

similar to if/else statement

```plsql
CASE
	WHEN condition1 THEN result1
	WHEN condition2 THEN result2
	ELSE some_other_result
END
```



```plsql
SELECT a,
	CASE WHEN a=1 THEN 'one'
		 WHEN a=2 THEN 'two'
    ELSE 'other' AS lable
    END
    FROM test;
```



we have another syntax (like switch statement)

```plsql
CASE expression
	WHEN value1 THEN result1
	WHEN value2 THEN result2
	WHEN value3 THEN result3
	ELSE some_other_result
END
```



```plsql
SELECT a 
	CASE a 
		WHEN 1 THEN 'one'
		WHEN 2 THEN 'two'
        ELSE 'other'
   END
FROM test;
```



```plsql
SELECT customer.email,
	CASE 
		WHEN customer_id < 100 THEN 'Premium'
		WHEN customer_id BETWEEN 100 AND 200 'Plus'
		ELSE 'Normal'
	END
FROM customer;
```



useful thing is to sum result 

```plsql
SELECT 
SUM(CASE rental_rate
   		WHEN 0.99 THEN 1
   		ELSE 0
   END) AS bargains,
SUM(CASE rental_rate
   		WHEN 2.99 THEN 1
   		ELSE 0
   END) AS regulat
SUM(CASE rental_rate,
    	WHEN 4.99 THEN 1
   		ELSE 0
   END) AS premium
FROM film;
```



sum rating for each rating movie

```plsql
SELECT 
SUM(CASE rating
   		WHEN 'R' THEN 1
   		ELSE 0
   END) AS R,
SUM(CASE rating
   		WHEN 'PG' THEN 1
   		ELSE 0
   END) AS PG,
SUM(CASE rating
    	WHEN 'PG-13' THEN 1
   		ELSE 0
   END) AS PG13
FROM film
```



#### COALESCE

accept many argument , return first argument that not null.

```plsql
SELECT COALESCE(1,2) -- 1
SELECT COALESCE(NULL,2,3) -- 2
```



if a field from column may be null , we can replace this with deafult value for the query

```plsql
SELECT item,(price - COALESCE(disscount,0)) AS final FROM table
```

if discount is null this will take 0.



#### CAST

cast type to another type

for example string to number.



```plsql
SELECT CAST('5' AS INTEGER)
--ANOTHER WAY
SELECT '5'::INTEGER

SELECT CAST(date AS TIMESTAMP)
```



#### NULLIF

take 2 inputs , return NULL if both are equal otherwise it returns the first argument passed.

`NULLIIF(10,10)` return null

`NULLIF(12,10)` return 12



#### Views

make shortcut for an query

```plsql
CREATE OR REPLACE VIEW customer_info AS
SELECT first_name,last_name,adress FROM customer
INNER JOIN adress
ON customer.address_id = adress.adress_id
```



now we can use it as

```plsql
SELECT * FROM customer_info
```



get views from table

```plsql
 SELECT table_name FROM INFORMATION_SCHEMA.views;
```



# Brief Other things

### Indexes

```plsql
explain SELECT * FROM table WHERE column_name='some-input'; 
-- will give you time execution

CREATE INDEX name_index on table_name(column_name)
explain SELECT * FROM table WHERE column_name='some-input'; 
-- may give better results, will cost in insertion.

DROP INDEX name_index
```



### Functions

for usability&readability we can create function 

```plsql
CREATE OR REPLACE FUNCTION total()
RETURNS INTEGER AS $total$
DECLARE 
total INTEGER;
BEGIN 
SELECT COUNT(*) INTO total from cd.bookings; 
return total; 
END;
$total$ LANGUAGE plpgsql;
```



we can call function 

```plsql
SELECT total()
```

function gets parameter

```plsql
CREATE OR REPLACE FUNCTION inc(value integer)
RETURN integer AS $inc$
BEGIN
RETURN value+1;
END;
$inc$ LANGUAGE plpgsql;
```

```plsql
SELECT inc(6) --return 7
```



#### Transaction

```plsql
begin;
DELETE FROM cd.bookings WHERE bookid=14;
end; --we either can use  rollback;

begin;
DELETE FROM cd.bookings WHERE bookid=14;
rollback;
```



### Trigger

function that will be called when event occur.

```plsql
CREATE OR REPLACE FUNCTION log_change()
returns trigger AS
$BODY$
BEGIN
if NEW.adress<>OLD.adress then
INSERT INTO emp_logs values(old.id,old.adress,new.adress,old.name)
end if;
return new;
END;
$BODY$
LANGUAGE plpgsql;
```

```plsql
CREATE trigger log_changer
BEFORE UPDATE ON compay
for each row
execute procedure log_chcage()
```

each change of company table will lead automatically to new information on  table `emp_logs`.



change name of trigger

```plsql
ALTER TRIGGER log_changer on company 
rename to log_changer_rename;
```



disable trigger

```plsql
ALTER TABLE company
DISABLE TRIGGER log_changer_reanme; 
-- if you want disable all trigger you can instead of name put all keyword.
```

















---

get all tables from PostgreSQL

```sql
SELECT table_name FROM information_schema.tables WHERE table_schema='public'

```



load tar to db

```bash
pg_restore --host localhost --port 5432 --username postgres --dbname dvdrental --role PostgreSQL --verbose /original.tar
```

