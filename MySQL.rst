#####
MySQL
#####

General
*******



Connect

.. code-block:: bash

  mysql -u <user_name> -p

Create

.. code-block:: sql

    CREATE DATABASE <db_name>

List databases

.. code-block:: sql

    SHOW DATABASES;

Use database

.. code-block:: sql

    USE <db_name>

Delete database

.. code-block:: sql

    DROP DATABASE <db_name>

Dump database

.. code-block:: bash

  mysqldump -p -u <username> <db_name> > backup-file.sql

Load from dump

.. code-block:: bash

  mysql <db_name> < backup-file.sql

When importing from a dump file, make sure that it does not contain a :code:`USE <database>` statement at the beginning. If it does, it might not update the database that is described in the mysql command above, but the one provided in the USE statement.
    

Users
*****

Create

.. code-block:: sql

  create user myuser@localhost identified by 'mypassword';

View

.. code-block:: sql

  select * from mysql.user;

Delete

.. code-block:: sql

  drop user myuser@localhost;

Change password

.. code-block:: sql

  alter user 'root'@'localhost' identified by 'mypassword';

Grant all privileges to a user on a specific database

.. code-block:: sql

  grant all privileges on `mydb`.* TO 'myuser'@localhost;

Must be logged in as root and have the database <db_name> loaded



Tables
******

Show all tables

.. code-block:: sql

  show tables;

Create table

.. code-block:: sql

   -- Create a table user in schema blog, with primary key 'id'
   CREATE TABLE `blog`.`user` (
      `id` INT UNSIGNED NOT NULL AUTO_INCREMENT,
      `firstname` VARCHAR(255) ,
      `lastname` VARCHAR(255) NOT NULL,
      PRIMARY KEY (`id`)
      );

   -- Create a table post with a foreign key 'user_id' referencing `user`.`id`
   CREATE TABLE `blog`.`post` (
      `id` INT UNSIGNED NOT NULL AUTO_INCREMENT,
      `text` VARCHAR(255),
      `user_id` INT UNSIGNED,
      CONSTRAINT pk_id PRIMARY KEY (`id`),
      CONSTRAINT fk_user_id FOREIGN KEY (`user_id`) REFERENCES `user` (`id`)
      );



Describe table

.. code-block:: sql

    DESCRIBE food;

Alter table

.. code-block:: sql

  ALTER TABLE t2 DROP COLUMN c, DROP COLUMN d;


Delete table

.. code-block:: sql

  drop table favorite_food; 

  

Data types

+--------------+-------------------------------+-----------+-------------------------------------------------------+
|  CHAR        | String (0-255)                | INT       | Integer (-2147483648 to 2147483647)                   |
+--------------+-------------------------------+-----------+-------------------------------------------------------+
|  VARCHAR     | String (0-255)                | BIGINT    | Integer (-9223372036854775808 to 9223372036854775807) |
+--------------+-------------------------------+-----------+-------------------------------------------------------+
|  TINYTEXT    | String (0-255)                | FLOAT     | Decimal (precise to 23 digits)                        |
+--------------+-------------------------------+-----------+-------------------------------------------------------+
|  TEXT        | String (0-65535)              | DOUBLE    | Decimal (24 to 53 digits)                             |
+--------------+-------------------------------+-----------+-------------------------------------------------------+
|  BLOB        | String (0-65535)              | DECIMAL   | "DOUBLE" stored as string                             |
+--------------+-------------------------------+-----------+-------------------------------------------------------+
|  MEDIUMTEXT  | String (0-16777215)           | DATE      | YYYY-MM-DD                                            |
+--------------+-------------------------------+-----------+-------------------------------------------------------+
|  MEDIUMBLOB  | String (0-16777215)           | DATETIME  | YYYY-MM-DD HH:MM:SS                                   |
+--------------+-------------------------------+-----------+-------------------------------------------------------+
|  LONGTEXT    | String (0-4294967295)         | TIMESTAMP | YYYMMDDHHMMSS                                         |
+--------------+-------------------------------+-----------+-------------------------------------------------------+
|  LONGBLOB    | String (0-4294967295)         | TIME      | HH:MM:SS                                              |
+--------------+-------------------------------+-----------+-------------------------------------------------------+
|  TINYINT     | Integer (-128 to 127)         | ENUM      | One of preset options                                 |
+--------------+-------------------------------+-----------+-------------------------------------------------------+
|  SMALLINT    | Integer (-23768 to 32767)     | SET       | Selection of preset options                           |
+--------------+-------------------------------+-----------+-------------------------------------------------------+
|  MEDIUMINT   | Integer (-8388608 to 8388607) | BOOLEAN   | TINYINT(1)                                            |
+--------------+-------------------------------+-----------+-------------------------------------------------------+



Inserting data
**************

Insert data

.. code-block:: sql

  insert into <table>
         (column1, column2, ...)
  values (col1val, col2val, ...);


Output data

.. code-block:: sql

  select column1, column2, ...
  from <table>
  where <condition> /*(e.g. lname = 'Smith')*/
  order by <ColumnName>


Update data

.. code-block:: sql

  update <table name>
  set colName1 = colValue1
      colName2 = colValue2
      colName3 = colValue3
  where <condition> /*e.g. person_id = 1*/


Delete data

.. code-block:: sql

  delete from <TableName>
  where <condition> /*e.g. person _id = 2*/


Reset auto increment

.. code-block:: sql

  alter table <table_name> auto_increment=1;




Chapter 3
*********

The select statement
====================

Columns can be transformed


.. code-block:: sql

  select column1, 
         func(column2), 
         column3 * x
  from <table>

Duplicate removal

.. code-block:: sql

    select distinct 

Aliases
=======

.. code-block:: sql

  select column1 alias1, column2 alias2
  from table1 alias1, table2 alias2     /* no comma between name and alias*/

or

.. code-block:: sql

  select column1 AS alias1, column2 AS alias2
  from table1 AS alias1, table2 AS alias2

Subqueries
==========

The from clause defines the tables used by a query, along with the means of
linking the tables together.  E.g.:

.. code-block:: sql

  select e.emp_id, e.fname, e.lname
  from (
        select emp_id, fname, lname, start_date, title
        from employee
       ) e;
    
There is a subquery within the parentheses, and the resulting table is 
called e. This table is then used for the outer query.

Views
=====

The result of a query can be assigned a name, (virtual table, or view).

.. code-block:: sql

  create view employee_vw as
      select emp_id, fname, lname,
             year(start_date) start_year
      from employee;
    
  select *
  from employee_vw;


Views can be deleted

.. code-block:: sql

  DELETE VIEW <view_name>;

Joins
=====

Here's how to join two tables:

.. code-block:: sql

  SELECT *
  FROM a INNER JOIN b
      ON a.a = b.b
      INNER JOIN c
      ON a.a = c.c;

For example:

.. code-block:: sql

  SELECT employee.emp_id, employee.fname,
         employee.lname, department.name dept_name
  FROM employee INNER JOIN department
       ON employee.dept_id = department.dept_id;


Where
=====

Where filters out rows. More than 1 conditions can be joined with and, or, not.


Group by
========

Join department and employee tables, group them by name, 
create a column that counts the instances and show those with more than 2 
instances only. 

.. code-block:: sql

  select d.name, count(e.emp_id) num_employees
      from department d inner join employee e
      on d.dept_id = e.dept_id
      group by d.name
      having count(e.emp_id) > 2;

:code:`HAVING` is used to check conditions after the aggregation takes places.
:code:`WHERE`  is used before the aggregation takes place.

Order by
========

.. code-block:: sql

  select *
  from table
  order by column1 desc, column2 asc;

ascending is the default and can be left out
need to specify desc for every column that needs sorting in descending order.

Order by can also take functions


Chapter 4
*********

Conditions
==========

.. code-block:: sql

  LIKE, IN, BETWEEN
  WHERE <column_name> IN ('a', 'b', 'c')
  WHERE <column_name> BETWEEN 300 AND 500
  WHERE <column_name> LIKE '_a%e'
      _ matches a single character
      % matches any number of characters
  WHERE <column_name> REGEXP 'regular expression'
    
Null
====

IS :code:`NULL` tests for null

don't test with :code:`=NULL` (Nulls don't equate)

When looking for rows that do NOT take a specific value, test for NULLs separately. 


Chapter 8 - Grouping
********************

Aggregate functions

.. code-block:: sql

  Max()
  Min()
  Avg()
  Sum()
  Count()
  GROUP BY 
  HAVING

Extras
******

Conditional values 
===================

.. code-block:: sql

  CASE WHEN <condition> 
       THEN <value for condition true> 
       ELSE <value for condition false> 
       END

Extract week from date
======================

.. code-block:: sql

  EXTRACT(week FROM date)

Translate into time zones
=========================

.. code-block:: sql

  time_value AT TIME ZONE 'PDT' > '2013-10-01 10:00'

Case insensitive
================

To make the table names case insensitive

Open the /etc/my.cnf and add in the [mysqld] section :code:`lower_case_table_names = 1`


Session variables
=================

.. code-block:: sql

  Set variable
  SET @x= 0;

When used inside statements values are assigned with :=, e.g. 

.. code-block:: sql

  @x := 0


Ranking according to groups
===========================

Let the table cities: 

.. code-block:: bash

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


In SQL ranking according to groups can be done by :code:`rank()` over :code:`(partition BY country ORDER BY population) rank`.

This will create a column called rank, that will include the rankings per group

In MySQL this doesn't work. 

We can instead do this:

.. code-block:: sql

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

This will return:

.. code-block:: bash

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

The innermost :code:`SELECT`, selects the whole cities table and orders it by 
country and population.

The 2 :code:`SELECTS` that preceed it initialise the session variables :code:`@country_rank` and :code:`@current_country`.  This step is necessary; if omitted the query will not work the first time it runs, but it will the second!

The 2nd innermost :code:`SELECT` creates a table that contains city, country, population and 2 more columns. The first is country_rank, which is an increasing number that resets every time a new country is encountered.  The last column is a storage of the current country, to be remembered when the next row is read.

Finally, the outermost :code:`SELECT` is needed to a) remove the uneccessary columns and b) if WHERE is used in the SELECT that's just below, the country_rank column is not seen. Not sure why...

The :code:`FROM (SELECT @country_rank := 0) crtemp`, statements could be 
replaced with an initailisation of the session variables outside the 
query as:

.. code-block:: sql

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


Change root password
====================

https://coolestguidesontheplanet.com/how-to-change-the-mysql-root-password/

1) Stop server

.. code-block:: bash

    sudo /usr/local/mysql/suuport-files/mysql.server stop 

(or from the system preferences)

2) Start in safe mode

.. code-block:: bash

    sudo mysqld_safe --skip-grant-tables

3) Open a new window

4) Start mysql as root

.. code-block:: sql

  FLUSH PRIVILEGES;
  ALTER USER 'root'@'localhost' IDENTIFIED BY 'NewPassword';

5) Kill the mysql process

6) Start the server

.. code-block:: bash

    sudo /usr/local/mysql/suuport-files/mysql.server start

7) Start mysql

.. code-block:: bash

    my sql -u root -p



SQL Server
**********

Specify the columns of a table

.. code-block:: sql

  sp_columns <table_name>


Install MariaDB on centos
*************************

Create MariaDB 

.. code-block:: bash

  sudo yum install mariadb-server
  sudo systemctl start mariadb
  sudo systemctl status mariadb
  sudo systemctl enable mariadb

Secure MariaDB

.. code-block:: bash

  sudo mysql_secure_installation

The first password is empty, enter a new password and leave the rest to defaults (Y).

Test the installation

.. code-block:: bash

  mysqladmin -u root -p version

Stop Restart

.. code-block:: sql

  sudo systemctl stop mariadb
  sudo systemctl restart mariadb




