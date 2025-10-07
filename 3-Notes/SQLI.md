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


##### Retrieving data from other database tables
