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



