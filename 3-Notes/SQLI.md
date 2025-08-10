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

## Relational Databases

A relational database is the most common type of database. It uses a schema, a template, to dictate the data structure stored in the database. For example, we can imagine a company that sells products to its customers having some form of stored knowledge about where those products go, to whom, and in what quantity. However, this is often done in the back-end and without obvious informing in the front-end. Different types of relational databases can be used for each approach. For example, the first table can store and display basic customer information, the second the number of products sold and their cost, and the third table to enumerate who bought those products and with what payment data.







## **Blind Injection**
Bind SQLI happen because there's a bug (**SQLI**) but it isn't show in the response  and that could happen because of the injection plan like sometimes it is sometime happen in cookie but we can know from how the response act like , let's say a website show welcome back if everything work , so from this we can do this 
```
…xyz' AND '1'='1 
…xyz' AND '1'='2
```

- The first of these values causes the query to return results, because the injected `AND '1'='1` condition is true. As a result, the "Welcome back" message is displayed.
- The second value causes the query to not return any results, because the injected condition is false. The "Welcome back" message is not displayed.

and we can use this payload
`' AND SUBSTRING((SELECT password FROM users WHERE username='administrator'),1,1) > m` if Welcomeback show up then first char is after `m` and if not it maybe be `m` or earlier (less) 