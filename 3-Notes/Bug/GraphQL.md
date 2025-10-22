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
Most `GraphQL` 