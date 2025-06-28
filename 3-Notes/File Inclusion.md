[[Room]]

web applications are written to request access to files on a given system, including images, static text, and so on via parameters. Parameters are query parameter strings attached to the URL that could be used to retrieve data or perform actions based on user input. The following diagram breaks down the essential parts of a URL.

 ![[dbf35cc4f35fde7a4327ad8b5a2ae2ec.png]]

- why it happen
	*in simple it vulnerability happen from the parameter and The main issue of these vulnerabilities is the input validation like in php if it poor written(as my writings here)*

- what the impact
	it can be use to leak like source code,credentials and others and if the hacker can write files he will be able to gain remote access to the server and this is the worst
(by this complete the first task , and the second it just deploying the machine)

## Path Traversal
it vulnerability allows the attacker (hacker) to read operation system resources and this could happen from parameter  
 ![[45d9c1baacda290c1f95858e27f740c9.png]]
 - why it happen
	 because of file_get_content in php that when the user input get by this so the attacke can manipulating 
- what the impact
	it allows the attacker to read operation system resources like /etc/passwd

it looks like this `http://webapp.thm/get.php?file=../../../../etc/passwd`

![[dot-dot-slash.png]]

on windows `http://webapp.thm/get.php?file=../../../../boot.ini`  or
`http://webapp.thm/get.php?file=../../../../windows/win.ini`

what file you can read and it will be usefull

|                               |                                                                                                                                                                   |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Location**                  | **Description**                                                                                                                                                   |
| `/etc/issue`                  | contains a message or system identification to be printed before the login prompt.                                                                                |
| `/etc/profile`                | controls system-wide default variables, such as Export variables, File creation mask (umask), Terminal types, Mail messages to indicate when new mail has arrived |
| `/proc/version`               | specifies the version of the Linux kernel                                                                                                                         |
| `etc/passwd`                  | has all registered users that have access to a system                                                                                                             |
| `/etc/shadow`                 | contains information about the system's users' passwords                                                                                                          |
| `/root/.bash_history`         | contains the history commands for `root` user                                                                                                                     |
| `/var/log/dmessage`           | contains global system messages, including the messages that are logged during system startup                                                                     |
| `/var/mail/root`              | all emails for `root` user                                                                                                                                        |
| `/root/.ssh/id_rsa`           | Private SSH keys for a root or any known valid user on the server                                                                                                 |
| `/var/log/apache2/access.log` | the accessed requests for `Apache` web server                                                                                                                     |
| `C:\boot.ini`                 | contains the boot options for computers with BIOS firmware                                                                                                        |

q/ what function can cause Path Traversal
a/

(by this task 3 end)

## Local File Inclusion (LFI)

it occur due to developer's leak of security awareness with php using functions such as `require`  `include` `require_once` `include_once`

**###1.** Suppose the web application provides two languages, and the user can select between the EN and AR
------------------------------------------------------------------

```php
<?PHP 
	include($_GET["lang"]);
?>
```

The PHP code above uses a GET request via the URL parameter lang to include the file of the page. The call can be done by sending the following HTTP request as follows: `http://webapp.thm/index.php?lang=EN.php` to load the English page or `http://webapp.thm/index.php?lang=AR.php` to load the Arabic page, where EN.php and AR.php files exist in the same directory.

Theoretically, we can access and display any readable file on the server from the code above if there isn't any input validation. Let's say we want to read the `/etc/passwd` file, which contains sensitive information about the users of the Linux operating system, we can try the following: `http://webapp.thm/get.php?file=/etc/passwd`

In this case, it works because there isn't a directory specified in the include function and no input validation.

and it ask for **lab** so i do it and it was just like path traversal, so the answer was just change the file parameter to /etc/passwd **like this** `http://webapp.thm/get.php?file=/etc/passwd`

**###2.** Next, In the following code, the developer decided to specify the directory inside the function.
------------------------------------------------------------------

```php
<?PHP 
	include("languages/". $_GET['lang']); 
?>
```

In the above code, the developer decided to use the include function to call PHP pages in the languages directory only via lang parameters.

If there is no input validation, the attacker can manipulate the URL by replacing the lang input with other OS-sensitive files such as /etc/passwd.

Again the payload looks similar to the path traversal, but the include function allows us to include any called files into the current page. The following will be the exploit:
`http://webapp.thm/index.php?lang=../../../../etc/passwd`

i do lab here and the ans was like this |^|

**(by this task 4 end)**

**#3.** In the first two cases, we checked the code for the web app, and then we knew how to exploit it. However, in this case, we are performing black box testing, in which we don't have the source code. In this case, errors are significant in understanding how the data is passed and processed into the web app.

in this case you don't know the source code , we write some wrong input to see the error that occur and maybe give us some info **(this itself is vulnerability)** and by that you know the function they use for example :
In this scenario, we have the following entry point: `http://webapp.thm/index.php?lang=EN`. If we enter an invalid input, such as THM, we get the following error

```php
Warning: include(languages/THM.php): failed to open stream: No such file or directory in /var/www/html/THM-4/index.php on line 12
```

The error message discloses significant information. By entering THM as input, an error message shows what the include function looks like: `include(languages/THM.php);`.
The error message discloses significant information. By entering THM as input, an error message shows what the include function looks like: `include(languages/THM.php);`.

If you look at the directory closely, we can tell the function includes files in the languages directory is adding .php at the end of the entry. Thus the valid input will be something as follows: `index.php?lang=EN`, where the file EN is located inside the given languages directory and named `EN.php`.

Also, the error message disclosed another important piece of information about the full web application directory path which is `/var/www/html/THM-4/`.

if we try to do this  `http://webapp.thm/index.php?lang=../../../../etc/passwd`
this will be the response :
```php
Warning: include(languages/../../../../../etc/passwd.php): failed to open stream: No such file or directory in /var/www/html/THM-4/index.php on line 12
```
by this we know that there is a fuction that check for `.php` so we can use something call null-byte (i think this don't work any more in real-world)  and this is the null-byte **`%00`**
Using null bytes is an injection technique where URL-encoded representation such as %00 or 0x00 in hex with user-supplied data to terminate strings. You could think of it as trying to trick the web app into disregarding whatever comes after the Null Byte.

By adding the Null Byte at the end of the payload, we tell the include function to ignore anything after the null byte which may look like:

`include("languages/../../../../../etc/passwd%00").".php");` which is equivalent to `include("languages/../../../../../etc/passwd");`

**Note:** the %00 trick is fixed and not working with PHP 5.3.4 and above.
**#5.** Next, in the following scenarios, the developer starts to use input validation by filtering some keywords. Let's test out and check the error message!

`http://webapp.thm/index.php?lang=../../../../etc/passwd`

We got the following error!

```php
Warning: include(languages/etc/passwd): failed to open stream: No such file or directory in /var/www/html/THM-5/index.php on line 15
```

If we check the warning message in the `include(languages/etc/passwd)` section, we know that the web application replaces the `../` with the empty string. There are a couple of techniques we can use to bypass this.

First, we can send the following payload to bypass it: `....//....//....//....//....//etc/passwd`.

Why did this work?

This works because the PHP filter only matches and replaces the first subset string `../` it finds and doesn't do another pass, leaving what is pictured below.

![[pathTRAVEL.png]]


Try out Lab #5 and try to read /etc/passwd and bypass the filter!

**#6.** Finally, we'll discuss the case where the developer forces the include to read from a defined directory! For example, if the web application asks to supply input that has to include a directory such as: `http://webapp.thm/index.php?lang=languages/EN.php` then, to exploit this, we need to include the directory in the payload like so: `?lang=languages/../../../../../etc/passwd`.