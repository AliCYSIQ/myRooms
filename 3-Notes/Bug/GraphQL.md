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

The example query below returns full details on all queries, mutations, subscriptions, types, and fragments.

```
# Full Introspection Query
query IntrospectionQuery {
  __schema {
    queryType {
      name
    }
    mutationType {
      name
    }
    subscriptionType {
      name
    }
    types {
      ...FullType
    }
    directives {
      name
      description
      args {
        ...InputValue
      }
      # The following fields often need to be removed if the server rejects them:
      onOperation
      onFragment
      onField
    }
  }
}

fragment FullType on __Type {
  kind
  name
  description
  fields(includeDeprecated: true) {
    name
    description
    args {
      ...InputValue
    }
    type {
      ...TypeRef
    }
    isDeprecated
    deprecationReason
  }
  inputFields {
    ...InputValue
  }
  interfaces {
    ...TypeRef
  }
  enumValues(includeDeprecated: true) {
    name
    description
    isDeprecated
    deprecationReason
  }
  possibleTypes {
    ...TypeRef
  }
}

fragment InputValue on __InputValue {
  name
  description
  type {
    ...TypeRef
  }
  defaultValue
}

fragment TypeRef on __Type {
  kind
  name
  ofType {
    kind
    name
    ofType {
      kind
      name
      ofType {
        kind
        name
      }
    }
  }
}

```

>  **Note**
> If introspection is enabled but the above query doesn't run, try removing the `onOperation`, `onFragment`, and `onField` directives from the query structure. Many endpoints do not accept these directives as part of an introspection query, and you can often have more success with introspection by removing them.

### Visualizing introspection results
You can view relationships between schema entities more easily using a [GraphQL visualizer](http://nathanrandal.com/graphql-visualizer/).
This is an online tool that takes the results of an introspection query and produces a visual representation of the returned data, including the relationships between operations and types.

### if introspection was disabled
here there's some ways to get information even if it it disabled

Suggestions are a feature of the Apollo GraphQL platform in which the server can suggest query amendments in error messages. These are generally used where a query is slightly incorrect but still recognizable (for example, `There is no entry for 'productInfo'. Did you mean 'productInformation' instead?`).

[Clairvoyance](https://github.com/nikitastupin/clairvoyance) is a tool that uses suggestions to automatically recover all or part of a GraphQL schema, even when introspection is disabled.

### lab
after i'm in the website , i found this in the burp history `/graphql/v1` which mean this website have `graphql`, send it to the repeater and check if introspection is enabled or no

after i send the query , it was enable , after see the result i found this `postpassword` .

before anything i found this website **have 5 blog post but the one with `id =3` is not visible** 

after send `graphql/v1` to the repeater and change the variable from `e.g. 5 to 3` and add `postpassword` to query panel , here you will get  the post password

