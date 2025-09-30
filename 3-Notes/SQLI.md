[[Bug]]

**first** every website has its database and when HTTP request send it will carry some queries and sent it as response , enabling the user to perform other unintended SQL queries directly against the database input and you can do this 

and to do that you need to inject single quote(**'**) or double quote (**"**) and followed by payload.

Once an attacker can inject, they have to look for a way to execute a different SQL query. This can be done using SQL code to make up a working query that executes both the intended and the new SQL queries. There are many ways to achieve this, like using [stacked](https://www.sqlinjection.net/stacked-queries/) queries or using [Union](https://www.mysqltutorial.org/sql-union-mysql.aspx/) queries. Finally, to retrieve our new query's output, we have to interpret it or capture it on the web application's front end.

## Database Management Systems

A Database Management System (DBMS) helps create, define, host, and manage databases. Various kinds of DBMS were designed over time, such as file-based, Relational DBMS (RDBMS), NoSQL, Graph based, and Key/Value stores.

## **impact**
the impact is too powerfull like it can lead to data breaches like **username** and **password** and maybe **credit card** and maybe bypass which lead to get content for premium only or get access to private information and maybe make the attacker be able to write and read files on back-end files and every info on the server or the website which lead to other attacks 

## Architecture

The diagram below details a two-tiered architecture.

![Three-tier architecture diagram: User interacts with a client application at Tier I, which connects to an application server at Tier II, and a DBMS at Tier III. Users and a database administrator access the DBMS.](https://academy.hackthebox.com/storage/modules/33/db_2.png)

`Tier I` usually consists of client-side applications such as websites or GUI programs. These applications consist of high-level interactions such as user login or commenting. The data from these interactions is passed to `Tier II` through API calls or other requests.

The second tier is the middleware, which interprets these events and puts them in a form required by the DBMS. Finally, the application layer uses specific libraries and drivers based on the type of DBMS to interact with them. The DBMS receives queries from the second tier and performs the requested operations. These operations could include insertion, retrieval, deletion, or updating of data. After processing, the DBMS returns any requested data or error codes in the event of invalid queries.

# Types of Databases



Databases, in general, are categorized into `Relational Databases` and `Non-Relational Databases`. Only Relational Databases utilize SQL, while Non-Relational databases utilize a variety of methods for communications.

# Relational databases
A relational database stores data in named tables that follow a schema (a structure that defines columns and types). Each table has columns (vertical fields) and rows (horizontal records). Tables should have unique names within the same database; columns should have unique names within their table. Data is linked across tables using keys (for example, a primary key in one table and a foreign key in another), which keeps relationships explicit and consistent.



### commands 

take some commands that will be helpful and sql commands are not case-sensitive and it should end with semicolon`;` 
#### **SELECT**
it use to retrieve data from database 
  
**`select * from users;`**
`select`: retrieve data from database
`*`: it mean what you want to retrieve and here it mean every thing
`from`:from mean from.
`users`: it mean the name of the table that you want to retrieve (select)
`;`: it use in the end of every command

**SO here it mean it will retrieve everything (all columns and rows) from users table **

`select username,password from users;`
which will mean retrieve username and password from 