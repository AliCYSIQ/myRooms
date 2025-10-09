[[Bug]]
source: [portswigger](https://portswigger.net/web-security/nosql-injection)

#### NoSQL syntax injection

try to enter some special  character and fuzz strings and look for errors.
every language have different special character so If you know the API language of the target database, use special characters and fuzz strings that are relevant to that language. and if not try to use different ones 
here i will focus on **`MongoDB`** language 

for example in website `https://insecure-website.com/product/lookup?category=fizz` it will send json query to retrieve data , you can check if it vulnerable or not using this payload
```
'"`{ 
;$Foo} 
$Foo \xYZ
```

will be like this `https://insecure-website.com/product/lookup?category='%22%60%7b%0d%0a%3b%24Foo%7d%0d%0a%24Foo%20%5cxYZ%00`

```
Note
NoSQL injection vulnerabilities can occur in a variety of contexts, and you need to adapt your fuzz strings accordingly. Otherwise, you may simply trigger validation errors that mean the application never executes your query.

In this example, we're injecting the fuzz string via the URL, so the string is URL-encoded. In some applications, you may need to inject your payload via a JSON property instead. In this case, this payload would become '\"`{\r;$Foo}\n$Foo \\xYZ\u0000
```
--------
-----
###### Determining which characters are processed
then try to inject each char alone for exmaple if you submit this `'` 
`this.category == '''`
if it cause error , then try this
`this.category == '\''`
to escape the quote 
if the error disappears then it vulnerable to injection attack  

---
###### Confirming conditional behavior
now lets see if  can inject boolean or no and to do that we will send two request one true and the other one is false and see if it response differently For example you could use the conditional statements **`' && 0 && 'x`** and **`' && 1 && 'x`** as follows:

**`https://insecure-website.com/product/lookup?category=fizzy'+%26%26+0+%26%26+'x`**

**`https://insecure-website.com/product/lookup?category=fizzy'+%26%26+1+%26%26+'x`**

check for different in response or behave 

----
