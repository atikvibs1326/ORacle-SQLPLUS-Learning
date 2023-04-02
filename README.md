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


