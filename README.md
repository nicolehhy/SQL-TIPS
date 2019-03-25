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



