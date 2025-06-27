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
