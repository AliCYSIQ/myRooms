[[Bug]]

## API Recon

you need to find APIs endpoint and gather these information:
1. The input data the API processes
2. HTTP methods that accept 
3. Rate limits and authentication mechanisms.
## API Documentation
search and read API documentation 
have two type : `human-readable` , `machine-readable` 
if it wasn't public , ways to find it:
1. crawl the API endpoints
   for example :
   - `/api`
   -  `/swagger/index.html`
   - `/openapi.json`
 2. investigate every thing in API , for example `/api/swagger/v1/users/123`, then you should investigate the following paths:
- `/api/swagger/v1`
-  `/api/swagger`
- `/api`

for machine-readable Documentation , should use tools like `opinAPI Parser` extension to analysis it 

**API Documentations are  not reliable always so using crawl and manual browsing is critical**

## Identifying API endpoints

use automation crawling and manual browsing , and look for any api endpoint like `/api/` and look inside js files using tools like JS Link Finder


### Interacting with API endpoints

try change HTTP methods and media type and look for the different in response 

#### Identifying supported HTTP methods

use `OPTIONS` to know what the methods that are accepted

use **`GET`** To get secret info or  **`POST`** to create one and **`DELETE`** to delete one (don't use delete at all for safety)

#### Identifying supported content types

testing **different type** of content may enable:
1. Errors that show information
2. Bypass defenses
3. be more vulnerable to different type of content 
to change content types , change the`Content-Type` header then reformat the request body accordingly. You can use the [Content type converter](https://portswigger.net/bappstore/db57ecbe2cb7446292a94aa6181c9278) BApp to automatically convert data

##### lab
found that product use API to know the price of every product 
`GET /api/products/1/price`
i only change it to `PATCH`
and add this 
```json
{
	"price":0
}

```
now it free for everyone. using another HTTP method  

#### Using tools to find hidden endpoints
like crawl tools , bruut force and 

## OWASP TOP 10 API Vuln

### Broken Object Level Authorization(BOLA)

it's completely like [[IDOR]] bug , 

for example :
```
/api/v1/suppliers/quarterly-reports/1
```

change it to :
```
/api/v1/suppliers/quarterly-reports/'i'
```

**i : any number** and it would retrieve sensitive info 

### Broken Authentication
it like [[authentication]] 

it about search for **brute force with out limit** on password, searching for sensitive info that could be found on the website 

the same but on **`OTP`**  

**in general search for any thing that could help you get unauthorized access to account**

###  Broken Object Property Level Authorization
it bug that let users access more info than they should/need, for example:

imagine there's marketplace which have customers and suppliers and they take **1$** fee.

if the customers were able to access info about suppliers more than they need it will be problem because:
1. direct access to suppliers so the customers get **discount** and and suppliers **avoid the fee**
2. could use their info to get access to their account using **brute-force** for example 
3.  etc...

### Improperly Controlled Modification of Dynamically-Determined Object Attributes

change the request body lead to change in the response, for example :

```json
...
{
	"isAadmin":0
}

```
here `0` mean `false` , so change it to `1`
will let you be admin
other examples:
```
{
	"price":25 

}
```
change it to `0` will let you get the product for free 

### Unrestricted Resource Consumption
this bug let you use **`RAM`, `STORAGE`, `CPU USING`, `NETWORK BANDWIDTH`**
which will cause cost loose to the company

let you upload files without limit to **how much size and how many times** you will upload
and that's will let you use all the storage in the server

able to send `sms OTP` or `email OTP` which will cause two things , high use of network bandwidth and annoying users

and this can apply on anything that don't have restrict on resource 

### Broken Function Level Authorization(BFLA)
if unauthorized or unprivileged can interact to privileged endpoint, able to run privileged function or information

The difference between `BOLA` and `BFLA` is that, in the case of `BOLA`, the user is authorized to interact with the vulnerable endpoint, whereas in the case of `BFLA`, the user is not.

for example :
admin can get billing address for all customers , if any unprivileged was able to get it too in any way , it will be **`BFLA`** 

### Unrestricted Access to Sensitive Business Flows
it's is the same as  `BFLA` 
with little bit of different 
### Server-Side Request Forgery

able to use the server it self to get access to files (not the only thing that **`SSRF`** can do , check **[[SSRF]]**)

for example :
user able to create product and can upload picture to this product and in able update the photo by **`URI`** to the photo , in normal **`file:///root/pic/product1.png`**
here you can update it and make it like this **`file:///etc/passwd`** or any file the server can access

### Security Misconfiguration

any Misconfiguration that can lead vulnerability ,accepts user-controlled input and incorporates it into SQL queries without proper validation, thereby allowing Injection attacks .
 for example :
#### Improper Neutralization of Special Elements used in an SQL Command ('SQL Injection') 

[[SQLI]]

### Improper Inventory Management

the user able to access old version ,deleted information , old endpoint , etc

for example :
could have access to old version and this version have some bugs , or info that shouldn't be access by users

###