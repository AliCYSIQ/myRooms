[[Network]]

source: [Web request on HTB](https://academy.hackthebox.com/module/35/section/219), 

HTTP stand for **`HyperText Transfer Protocol`** , The term **`hypertext`** stands for text containing links to other resources and text that the readers can easily interpret.

 ## URL

Resources over HTTP are accessed via a `URL`, which offers many more specifications than simply specifying a website we want to visit. Let's look at the structure of a URL: ![Diagram of a URL structure: scheme 'http', user 'admin:password', host 'inlanefreight.com', port '80', path '/dashboard.php', query string 'login=true', fragment 'status'.](https://academy.hackthebox.com/storage/modules/35/url_structure.png)

Here is what each component stands for:

| **Component**  | **Example**          | **Description**                                                                                                                                                                       |
| -------------- | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Scheme`       | `http://` `https://` | This is used to identify the protocol being accessed by the client, and ends with a colon and a double slash (`://`)                                                                  |
| `User Info`    | `admin:password@`    | This is an optional component that contains the credentials (separated by a colon `:`) used to authenticate to the host, and is separated from the host with an at sign (`@`)         |
| `Host`         | `inlanefreight.com`  | The host signifies the resource location. This can be a hostname or an IP address                                                                                                     |
| `Port`         | `:80`                | The `Port` is separated from the `Host` by a colon (`:`). If no port is specified, `http` schemes default to port `80` and `https` default to port `443`                              |
| `Path`         | `/dashboard.php`     | This points to the resource being accessed, which can be a file or a folder. If there is no path specified, the server returns the default index (e.g. `index.html`).                 |
| `Query String` | `?login=true`        | The query string starts with a question mark (`?`), and consists of a parameter (e.g. `login`) and a value (e.g. `true`). Multiple parameters can be separated by an ampersand (`&`). |
| `Fragments`    | `#status`            | Fragments are processed by the browsers on the client-side to locate sections within the primary resource (e.g. a header or section on the page).                                     |

Not all components are required to access a resource. The main mandatory fields are the scheme and the host, without which the request would have no resource to request.

## HTTPS

it stand for **Hypertext Transfer Protocol Secure**
it's the same as **HTTP** BUT  this is more secure because it encrypt every request so no one will see the request in clear text but the user can
 and here how it work 

![[Pasted image 20251009232252.png]]