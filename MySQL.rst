#####
MySQL
#####

--------------------------------------------------------------------------------
                                      General
--------------------------------------------------------------------------------
username lrngsql
password xyz
mysql -u lrngsql -p

--------------------------------------------------------------------------------
                                      Change Password 
--------------------------------------------------------------------------------
--- Change root password ---
https://coolestguidesontheplanet.com/how-to-change-the-mysql-root-password/

1) Stop server
    > sudo /usr/local/mysql/suuport-files/mysql.server stop 
    (or from the system preferences)

2) Start in safe mode
    >sudo mysqld_safe --skip-grant-tables

3) Open a new window

4) Start mysql as root
    >mysql -u root
    >FLUSH PRIVILEGES;
    >ALTER USER 'root'@'localhost' IDENTIFIED BY 'NewPassword';
    >\q

5) Kill the mysql process

6) Start the server
    >sudo /usr/local/mysql/suuport-files/mysql.server start

7) Start mysql
    >my sql -u root -p


--------------------------------------------------------------------------------
                               Databases 
--------------------------------------------------------------------------------
Create
    CREATE DATABASE <database_name>
Use database
    USE <database_name>
List databases
    SHOW DATABASES;
List databases
    SHOW DATABASES;
Grant rights to user
    GRANT ALL ON <db_name> to 'user_name'@'localhost'; 
    Must be logged in as root and have the database <db_name> loaded
Delete database
    DROP DATABASE <db_name>

    

--------------------------------------------------------------------------------
                               Creating tables                               
--------------------------------------------------------------------------------
--- Create table ---
CREATE TABLE favorite_food
    (person_id SMALLINT UNSIGNED,
     food VARCHAR(20),
     CONSTRAINT pk_favorite_food PRIMARY KEY (person_id, food),
     CONSTRAINT fk_fav_food_person_id FOREIGN KEY (person_id)
     REFERENCES person (person_id)
    );

--- Show all tables ---
show tables;

--- Delete table ---
drop table favorite_food; 



--------------------------------------------------------------------------------
                               Inserting data                                
--------------------------------------------------------------------------------
--- Insert data ---
insert into <table>
       (column1, column2, ...)
values (col1val, col2val, ...);


--- Output data ---
select column1, column2, ...
from <table>
where <condition> /*(e.g. lname = 'Smith')*/
order by <ColumnName>


--- Update data ---
update <table name>
set colName1 = colValue1
    colName2 = colValue2
    colName3 = colValue3
where <condition> /*e.g. person_id = 1*/


--- Delete data ---
delete from <TableName>
where <condition> /*e.g. person _id = 2*/


--------------------------------------------------------------------------------
                               Chapter 3                                     
--------------------------------------------------------------------------------
--- The select statement ---
Columns can be transformed
    >select column1, 
    >       func(column2), 
    >       column3 * x
    >from <table>

Duplicate removal
    select distinct 

--- Aliases ---   
select column1 alias1, column2 alias2
from table1 alias1, table2 alias2     /* no comma between name and alias*/

or

select column1 AS alias1, column2 AS alias2
from table1 AS alias1, table2 AS alias2

--- Subqueries ---
The from clause defines the tables used by a query, along with the means of
linking the tables together.  E.g.:
    >select e.emp_id, e.fname, e.lname
    >from (
    >      select emp_id, fname, lname, start_date, title
    >      from employee
    >     ) e;
    
There is a subquery within the parentheses, and the resulting table is 
called e. This table is then used for the outer query.

--- Views ---
The result of a query can be assigned a name, (virtual table, or view).
    >create view employee_vw as
    >    select emp_id, fname, lname,
    >           year(start_date) start_year
    >    from employee;
    
    >select *
    >from employee_vw;

Views can be deleted
    > DELETE VIEW <view_name>;

--- Joins ---
Here's how to join two tables:
    >select employee.emp_id, employee.fname,
    >       employee.lname, department.name dept_name
    >from employee inner join department
    >     on employee.dept_id = department.dept_id;


--- Where ---
Where filters out rows. More than 1 conditions can be joined with and, or, not.


--- Group by ---
Join department and employee tables, group them by name, 
create a column that counts the instances and show those with more than 2 
instances only. 
    >select d.name, count(e.emp_id) num_employees
    >    from department d inner join employee e
    >    on d.dept_id = e.dept_id
    >    group by d.name
    >    having count(e.emp_id) > 2;

HAVING is used to check conditions after the aggregation takes places.
WHERE  is used before the aggregation takes place.

--- Order by ---
select *
from table
order by column1 desc, column2 asc;

ascending is the default and can be left out
need to specify desc for every column that needs sorting in descending order.

Order by can also take functions


--------------------------------------------------------------------------------
                               Chapter 4                                     
--------------------------------------------------------------------------------
Conditions
    LIKE, IN, BETWEEN
    WHERE <column_name> IN ('a', 'b', 'c')
    WHERE <column_name> BETWEEN 300 AND 500
    WHERE <column_name> LIKE '_a%e'
        _ matches a single character
        % matches any number of characters
    WHERE <column_name> REGEXP 'regular expression'
    
Null
    IS NULL tests for null
    don't test with =NULL (Nulls don't equate)
    When looking for rows that do NOT take a specific value, test for NULLs 
        separately. 

--------------------------------------------------------------------------------
                               Chapter 5                                     
--------------------------------------------------------------------------------
SELECT
FROM a INNER JOIN b
    ON a.a = b.b
    INNER JOIN c
    ON a.a = c.c


--------------------------------------------------------------------------------
                               Chapter 8 - Grouping                          
--------------------------------------------------------------------------------
Aggregate functions
Max()
Min()
Avg()
Sum()
Count()

GROUP BY 
HAVING

--------------------------------------------------------------------------------
                               Extras                                        
--------------------------------------------------------------------------------
Give conditional values 
    CASE WHEN <condition> 
         THEN <value for condition true> 
         ELSE <value for condition false> 
         END

Extract week from date
    EXTRACT(week FROM date)

Translate into time zones
    time_value AT TIME ZONE 'PDT' > '2013-10-01 10:00'


--------------------------------------------------------------------------------
                         Session variables 
--------------------------------------------------------------------------------
Set variable
SET @x= 0;
When used inside statements values are assigned with :=, e.g. 
@x := 0


--------------------------------------------------------------------------------
                         Ranking according to groups                   
--------------------------------------------------------------------------------
Let the table cities: 
+-------------+----------------+------------+
| city        | country        | population |
+-------------+----------------+------------+
| Paris       | France         |    2181000 |
| Marseille   | France         |     808000 |
| Lyon        | France         |     422000 |
| London      | United Kingdom |    7825300 |
| Birmingham  | United Kingdom |    1016800 |
| Leeds       | United Kingdom |     770800 |
| New York    | United States  |    8175133 |
| Los Angeles | United States  |    3792621 |
| Chicago     | United States  |    2695598 |
+-------------+----------------+------------+
In SQL ranking according to groups can be done by
rank() over (partition BY country ORDER BY population) rank

This will create a column called rank, that will include the rankings per group

In MySQL this doesn't work. 

We can instead do this:

SELECT city, country, population
FROM (SELECT city, country, population,
            @country_rank := CASE WHEN @current_country = country 
                THEN @country_rank+1 ELSE 1 END country_rank,
            @current_country := country
      FROM (SELECT @country_rank := 0) crtemp,
           (SELECT @current_country := NULL) cctemp,
           (SELECT *
            FROM cities
            ORDER BY country, population DESC
           ) r1
      ) r2
WHERE country_rank <= 2;

This will return 
+-------------+----------------+------------+
| city        | country        | population |
+-------------+----------------+------------+
| Paris       | France         |    2181000 |
| Marseille   | France         |     808000 |
| London      | United Kingdom |    7825300 |
| Birmingham  | United Kingdom |    1016800 |
| New York    | United States  |    8175133 |
| Los Angeles | United States  |    3792621 |
+-------------+----------------+------------+


A lot is happening here!
The innermost SELECT, selects the whole cities table and orders it by 
country and population.

The 2 SELECTS that preceed it initialise the session variables @country_rank
and @current_country. This step is necessary; if ommited the query will not 
work the first time it runs, but it will the second!

The 2nd innermost SELECT creates a table that contains city, country, 
population and 2 more columns. The first is country_rank, which is an 
increasing number that resets every time a new country is encountered. 
The last column is a storage of the current country, to be remembered when
the next row is read.

Finally, the outermost SELECT is needed to a) remove the uneccessary columns
and b) if WHERE is used in the SELECT that's just below, the country_rank
column is not seen. Not sure why...

The FROM (SELECT @country_rank := 0) crtemp, statements could be 
replaced with an initailisation of the session variables outside the 
query as:

SET @country_rank = 0;
SET @current_country = NULL;
SELECT city, country, population
FROM (SELECT city, country, population,
            @country_rank := CASE WHEN @current_country = country 
                THEN @country_rank+1 ELSE 1 END country_rank,
            @current_country := country
      FROM (SELECT *
            FROM cities
            ORDER BY country, population DESC
           ) r1
      ) r2
WHERE country_rank <= 2;



--------------------------------------------------------------------------------
--- SQL Server -----------------------------------------------------------------
--------------------------------------------------------------------------------
sp_columns <table_name>
    Specify the columns of a table
