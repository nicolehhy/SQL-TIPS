## SQL-TIPS
There are some tips we should keep in mind when we use SQL language

### 1. Tips in `SELECT` ( When we hvae subquery, we should use where to join two different tables)
  * We can select COUNT() -/+ COUNT() in SQL
  * We can select an output of subquery as our new columns 
  ``` SQL
  SELECT countries.name AS country, 
(SELECT COUNT(*) 
  FROM cities
    WHERE countries.code = cities.country_code)   # make sure we join two tables using 'where'
AS cities_num.  # new column from another table
FROM countries
ORDER BY cities_num DESC, country
LIMIT 9;
```
  * Subquery can be a new table 
  ``` SQL
  SELECT p.code,local_name,p.lang_num
  From countries,    # table 1
  	-- Subquery (alias as subquery)
  	(SELECT code,count(*) AS lang_num
  	 From languages
  	 GROUP BY code) AS p.  # table 2
  -- Where codes match
  WHERE countries.code = p.code  # use where to limit the data
-- Order by descending number of languages
Order by p.lang_num DESC;
```
  
  * Subquery can be a condition when we use WHERE
  ``` SQL
  -- Select fields
SELECT name, continent, inflation_rate
  -- From countries
FROM countries
	-- Join to economies
INNER JOIN economies
	-- Match on code
ON countries.code = economies.code
  -- Where year is 2015
WHERE year = 2015
  -- And inflation rate in subquery (alias as subquery)
AND inflation_rate IN (
        SELECT MAX(inflation_rate) AS max_inf
        FROM (
             SELECT name, continent, inflation_rate
             FROM countries
             INNER JOIN economies
             ON countries.code = economies.code
             WHERE year = 2015) AS subquery
        GROUP BY continent);           # here we use WHERE ... IN (subquery) to filter our data
```

### 2. Tips in `WHERE` ( When we need to filter the results, we will probably use 'where' clause)
  * Where clause cannot be used with Aggregate functions(`AVG`,`MIN`,`MAX`,`MEAN`,`SUM`,`COUNT`)
  * Sometimes we use where along with these signals
  ``` SQL
  = #equal
<> #not equal
< #less than
> #greater than
<= #less than or equal to
>= #greater than or equal to 
```
  * When we have `AND` and `OR` in the where clause, be sure to enclose the individual clauses in parentheses, like so:
  ``` SQL
  SELECT title
  FROM films
  WHERE (release_year = 1994 OR release_year = 1995)  # here we can also use ' where year between 1994 and 1995' 
  AND (certification = 'PG' OR certification = 'R');
  ```

### 3. Tips in missing value
  * Sometimes, we need to query our data without NA, using `IS NULL` / `IS NOT NULL`
``` SQL
SELECT COUNT(*)
FROM people
WHERE birthdate IS/ IS NOT NULL;
```
  * `COUNT()` doesn’t count the missing value of the entity you choose

### 4. Tips in `UNION` and `UNION ALL`
  * Union: join two tables but it will delete the replicates with order
  * Union All: just bind two tables together, including everything
  
### 5. Tips in `UNION`, `INTERSECT`, `EXCEPT`, `MINUS` and `USING`
we have to make sure the name of the fields we choose are the same among the tables we choose from, then we can use these command in our script

### 6. Tips in Scalar function 
   * Sometimes, we use mod(x,y) to filter our data(eg: even or odd number)
   ``` SQL
   SELECT name 
   FROM city
   WHERE mod(code,2) = 1;
   ```
   * The other scalar functions could be `UCASE()`, `NOW()`,`ROUND()`

SQL statements
===
### The most commonly used SQL statements.

#### Basically used SQL statements
* QUERYING DATA FROM A TABLE
  * This part involves `Select` `Where` `Group by` `Having` `Order by` `Limit` usages.
  * This usages help us filter the data
  * Their order should be like :
    >SELECT
    >>WHERE
    >>>GROUP BY 
    >>>> HAVING
    >>>>> ORDER BY

* QUERYING FROM MULTIPLE TABLES
  * This part involves `Join/Inner Join` `Left Join` `Right Join` `Full Outer Join` `Cross Join` usages
  * This usages help us link multiple tables

* USING SQL OPERATORS
  * This part involves `Union` `Except` `Intersect` `Minus` usages.
  * This usages help us filter or bind the information we want efficiently
  * Keep in mind that when use this command, the entities of these tables should be the same

* MANAGING TABLES
  * This part involves `Create Table` `Alter *table Drop` `Alter *table Add` `Alter *table Rename` usages
  * This usages help us manage the tables in the database. We can delete/add the entities or rename them

*  MODIFYING DATA
    * Insert multiple rows into a table
``` SQL
 INSERT INTO t(column_list) 
 VALUES (value_list),
(value_list), ....;
```
   * Insert rows from t2 into t1
``` SQL
 INSERT INTO t1(column_list) 
 SELECT column_list
FROM t2;
```
   * Delete subset of rows in a table
``` SQL
  DELETE FROM t
WHERE condition;
```
#### The best interview questions
Frequently asked SQL Interview Questions with detailed answers and examples.Here are many sample SQL Interview questions in a few PDF files and their answers are given just below to them. If you need them, just download the PDF files in this SQL repository.


