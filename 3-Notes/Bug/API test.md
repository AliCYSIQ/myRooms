[[Bug]]

#### API Recon

you need to find APIs endpoint and gather this information:
1. The input data the API processes
2. HTTP methods that accept 
3. Rate limits and authentication mechanisms.
#### API Documentation
search and read API documentation 

if it wasn't public , ways to find it:
1. crawl the API endpoints
   for example :
   - `/api`
   -  `/swagger/index.html`
   - `/openapi.json`
 2. investigate every thing in API ,`/api/swagger/v1/users/123`, then you should investigate the following paths:
- `/api/swagger/v1`
- `/api/swagger`
- `/api`
