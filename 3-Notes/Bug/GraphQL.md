[[Bug]]

## Finding `GraphQL` endpoints

### Universal queries

send `query{__typename}` to any `GraphQL` endpoint, it will include the string `{"data": {"__typename": "query"}}` somewhere in its response

a way to know if it `graphql` or not

### Common endpoint names

- `/graphql`
- `/api`
- `/api/graphql`
- `/graphql/api`
- `/graphql/graphql`
add `v1` if any of these didn't catch it

>**NOTE**
>`GraphQL` services will often respond to any `non-GraphQL` request with a "query not present" or similar error.

### Request methods
Most `GraphQL` accepts `POST` method but trying `GET` or chaining content type that could have some vulnerability

If you can't find the `GraphQL` endpoint by sending POST requests to common endpoints, try resending the universal query using alternative HTTP methods.

## Exploiting unsanitized arguments

For example, the query below requests a product list for an online shop:

```
#Example product query
query { 
	products { 
		id 
		name 
		listed 
	} 
}
```

The product list returned contains only listed products.

```
#Example product response 
{ 
	"data": { 
		"products": [ 
			{ 
				"id": 1,
				 "name": "Product 1", 
				 "listed": true
			}, 
			{ 
				"id": 2, 
				"name": "Product 2", 
				"listed": true 
			}, 
			{ 
				"id": 4, 
				"name": "Product 4", 
				"listed": true 
			} 
		] 
	} 
}
```

From this information, we can infer the following:

- Products are assigned a sequential ID.
- Product ID 3 is missing from the list, possibly because it has been delisted.

```
#Query to get missing product 
	query { 
		product(id: 3) { 
			id 
			name
			listed
		} 
	}
```

this will let you get unlisted product

## Discovering schema information

using build-in `GraphQL` function to get information about schema like Introspection

### Using introspection

Introspection helps you to understand how you can interact with a GraphQL API. It can also disclose potentially sensitive data, such as description fields.

query the `__schema` field. This field is available on the root type of all queries.

you can let return only the available fields names

### Probing for introspection

they can enable introspection or disable it ,
 if it was enable :
```
#Introspection probe request 
{ 
	"query": "{__schema{queryType{name}}}" 
}
```

the response returns the names of all available queries.

### Running a full introspection query

