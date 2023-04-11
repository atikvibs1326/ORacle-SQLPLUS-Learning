# ORACLE-SQLPLUS
Database:
database is a collection of related data

Sql:
Structured query language is a language used to communicated with the database for Crud Operations.

CRUD:
CRUD stand for Create,Read,Update,Delete this are the setof operations that user can perform on a database by using SQL 

DBMS:
Data base managment system is that is a software that is used  to access and manipulate the data 

RDMS:
rdbms is a type of dbms where it store the data in the form of tabular format which groupg related data together

Difference between SQL and MYSQL
	SQL :
	Sql stands for structure query language also it is a language from which we acces the data way to Query 
	whereas,
	MYSQL:
	MYSQL:is an rdbms software that is used to query the rdbms data,actuall accessing of data is done over here
	we can perform CRUD operations on the data via MYSQL.

Datatypes in SQl:
Sql provides datatypes that that specify whattype of data can be stored in the particular column
-INT -Stores a number
 Number-Stores a number
 
 
 
 ntroduction to the Oracle subquery
A subquery is a SELECT statement nested inside another statement such as SELECT, INSERT, UPDATE, or DELETE. Typically, you can use a subquery anywhere that you use an expression.

Consider this following subquery example that uses the products table from the sample database.

products table
The following query uses the MAX() function to return the highest list price from the products table:

SELECT
    MAX( list_price )
FROM
    products;
Code language: SQL (Structured Query Language) (sql)
Orcle Subquery - max list price
To select the detailed information of the most expensive products, you use the list price above (8867.99) in the following query:

SELECT
    product_id,
    product_name,
    list_price
FROM
    products
WHERE
    list_price = 8867.99;
Code language: SQL (Structured Query Language) (sql)
Orcle Subquery example
As you can see, we need to execute two queries separately to get the most expensive product information. By using a subquery, we can nest the first query inside the second one as shown in the following statement:

SELECT
    product_id,
    product_name,
    list_price
FROM
    products
WHERE
    list_price = (
        SELECT
            MAX( list_price )
        FROM
            products
    );
Code language: SQL (Structured Query Language) (sql)
In this example, the query that retrieves the max price is called the subquery and the query that selects the detailed product data is called the outer query. We say that the subquery is nested within the outer query. Note that a subquery must appear within parentheses ().

Oracle evaluates the whole query above in two steps:

First, execute the subquery.
Second, use the result of the subquery in the outer query.
A subquery which is nested within the FROM clause of the SELECT statement is called an inline view. Note that other RDBMS such as MySQL and PostgreSQL use the term derived table instead of the inline view.

A subquery nested in the WHERE clause of the SELECT statement is called a nested subquery.

A subquery can contain another subquery. Oracle allows you to have an unlimited number of subquery levels in the FROM clause of the top-level query and up to 255 subquery levels in the WHERE clause.

Advantages of Oracle subqueries
These are the main advantages of subqueries:

Provide an alternative way to query data that would require complex joins and unions.
Make the complex queries more readable.
Allow a complex query to be structured in a way that it is possible to isolate each part.
Oracle Subquery examples
A) Oracle subquery in the SELECT clause example
The following statement returns the product name, list price, and the average list prices of products according to their categories:

SELECT
    product_name,
    list_price,
    ROUND(
        (
            SELECT
                AVG( list_price )
            FROM
                products p1
            WHERE
                p1. category_id = p2.category_id
        ),
        2
    ) avg_list_price
FROM
    products p2
ORDER BY
    product_name;
Code language: SQL (Structured Query Language) (sql)
Orcle Subquery in SELECT clause example
In this example, we used a subquery in the SELECT clause to get the average product’s list price. Oracle evaluates the subquery for each row selected by the outer query.

This subquery is called a correlated subquery which we will cover in detail in the next tutorial.

B) Oracle subquery in the FROM clause example
A subquery in the FROM clause of a SELECT statement is called an inline view which has the following syntax:

SELECT * FROM (subquery) [AS] inline_view;
Code language: SQL (Structured Query Language) (sql)
For example, the following statement returns the top 10 orders with the highest values:

SELECT
    order_id,
    order_value
FROM
    (
        SELECT
            order_id,
            SUM( quantity * unit_price ) order_value
        FROM
            order_items
        GROUP BY
            order_id
        ORDER BY
            order_value DESC
    )
FETCH FIRST 10 ROWS ONLY; 
Code language: SQL (Structured Query Language) (sql)
Orcle Subquery in FROM clause example
In this statement:

First, the subquery returns the list of order_id and order_value sorted by the order_value in descending order.
Then, the outer query retrieves the first 10 rows from the top of the list.
C) Oracle subquery with comparison operators example
The subqueries that use comparison operators e..g, >, >=, <, <=, <>, = often include aggregate functions, because an aggregate function returns a single value that can be used for comparison in the WHERE clause of the outer query.

For example, the following query finds products whose list price is greater than the average list price.

SELECT
    product_id,
    product_name,
    list_price
FROM
    products
WHERE
    list_price > (
        SELECT
            AVG( list_price )
        FROM
            products
    )
ORDER BY
    product_name;
Code language: SQL (Structured Query Language) (sql)
Orcle Subquery with comparison operator example
This query works as follows:

First, the subquery returns the average list price of all products.
Second, the outer query gets the products whose list price is greater than the average list price returned by the subquery.
D) Oracle subquery with IN and NOT IN operators
The subquery that uses the IN operator often returns a list of zero or more values. After the subquery returns the result set, the outer query makes uses of them.

See the following employees, orders, and order_items tables from the sample database.

For example, the following query finds the salesman who has sales above 100K in 2017:

SELECT
    employee_id,
    first_name,
    last_name
FROM
    employees
WHERE
    employee_id IN(
        SELECT
            salesman_id
        FROM
            orders
        INNER JOIN order_items
                USING(order_id)
        WHERE
            status = 'Shipped'
        GROUP BY
            salesman_id,
            EXTRACT(
                YEAR
            FROM
                order_date
            )
        HAVING
            SUM( quantity * unit_price )  >= 1000000  
            AND EXTRACT(
                YEAR
            FROM
                order_date) = 2017
            AND salesman_id IS NOT NULL
    )
ORDER BY
    first_name,
    last_name;
Code language: SQL (Structured Query Language) (sql)
Orcle Subquery with IN operator example
Oracle evaluates this query in two steps:

First, the subquery returns a list of the salesman whose sales is greater than or equal to 1 million.
Second, the outer query uses the salesman id list to query data from the employees table.
The following statement finds all customers who have not yet placed an order in 2017:

SELECT
    name
FROM
    customers
WHERE
    customer_id NOT IN(
        SELECT
            customer_id
        FROM
            orders
        WHERE
            EXTRACT(
                YEAR
            FROM
                order_date) = 2017
            
    )
ORDER BY
    name; 
Code language: SQL (Structured Query Language) (sql)
Orcle Subquery with NOT IN operator example
In this statement:

First, the subquery returns a list of ids of customers who placed one or more orders in 2017.
Second, the outer query returns the customers with the id that are not in the list returned by the subquery.
In this tutorial, you have learned about the Oracle subquery which gives you an alternative way to construct more readable queries without using complex joins or unions.

-VARCHAR -Stores series of characters strings,Symbols,numbers in it
 VARCHAR2 -this is the syntax in the oracle
 it is variable datatype that means it uses only limited bits that user need  varchar is more storage efficient

-CHAR -Stores only series of the character which is Strings
  it is a fixed datatype that means it has a fixed storage of 255 bytes
  char is not so storage efficient

-UNSIGNED AND SIGNED
	for Eg:
		tinyint ->(-128 to 127)
	unsigned tinyint ->(0-255)

-Types of SQL Commands
	
-DDL (Data Defination Language)
	ddl is used to the create a structure for creating database.
		-CREATE
		  create table ,Database,View.
		-ALTER
		add,modify,drop,delete ,rename table.
		-DROP
		 delete table,view,this deletes whole table that means the structure of the table will also
		 be deleted.
		-RENAME
		 rename databse,tablename,column name,etc
		-TRUNCATE
		 used to delete records rows,not the whole table but the data inside that table 
		 the stucture remains as it is.

-DQL(data query language)
	-SELECT 


-DML(Data manipulation Language)
	dml is used to manipulate or modify the data .
	-INSERT
		this command is used to insert data into tables and columns

	-UPDATE
		this commnd is used to update the relation in the table
	-DELETE
		this command is specificly used to delte rows in the table
	-MERGE
	 merge command select data from one sources table and updates or insert into a target table 
       the merge statement allows you to specify a condition to determine whether to update data 		
	 from or insert data into target table .
		Syntax:
			MERGE INTO target_table 
		
USING source_table 
ON search_condition
    WHEN MATCHED THEN
        UPDATE SET col1 = value1, col2 = value2,...
        WHERE <update_condition>
        [DELETE WHERE <delete_condition>]
    WHEN NOT MATCHED THEN
        INSERT (col1,col2,...)
        values(value1,value2,...)
        WHERE <insert_condition>; 

-DCL(Data Control language)
	dcl provides the authorities to the user
	-GRANT
		the grant command is used to give access privileges to the Databse
	
	-REVOKE
		this is used to revoke means remove some access privileges to the databse
		
<h2>Key Constraint in SQL</h2>
-----------------------------------------------------------------------------------------------------------------------------------------------------
-Foreign Key
	Foreign key is used to refer the primary key of another table or the same table to access it in the other table or the same table.
	it has referential integrity that may cause some violation while performing insertion,updation and deletion operations on the tables
	below are given two mimages that specify how referential integrity is maintained by the Sql by using the foreign key constraint.
	



	-Referntial integrity on the refrenced table that is performed on the base table which has the primary key

![image](https://user-images.githubusercontent.com/64660852/229362071-7598cc27-7712-4311-9b7a-eae10ffba764.png)

	-Referential integrity on the referncing table that is performed on the child table which is having the foreign key
![image](https://user-images.githubusercontent.com/64660852/229362445-4880b95a-9f2b-437f-8554-6ce196cffb5a.png)

	Syntax 
		column_name  datatype REFERENCES table_name(the acutual coloumn name) 
	-----------
	the underlined column name can be anything,but it is better to give the same name which is present in the primary key column from the base table
	
   Below image contains the example of Foreign key
	
![image](https://user-images.githubusercontent.com/64660852/229362820-8314951c-6dba-4a3a-a403-5cbd73069c89.png)

![image](https://user-images.githubusercontent.com/64660852/229362851-b554655e-ac82-4163-bfd4-316ed01aec6c.png)

![image](https://user-images.githubusercontent.com/64660852/229362865-9627a63c-f11b-499d-b0c9-ae569e59e9c1.png)




