[[HTB-LABS]] [[Week4]]

# **Appointment**
what i learned from it , is:
1. sql stands for Structured Query Language
2. the most fame bug is injection
3. http on p 80, https on p 443
4. if the page not found , the response will be `404`
5. in `gobuster` tool there's two modes one for directory and the other one for subdomain , to choose directory more you should `gobuster dir ...` this the way
6. in MySQL if you want to add comment you can do this by add **`#`** then the comment to use this in real-world like in log-in , sometimes it will look like this 
   `name=admin&password=1234456` you can change this and let it like this
   `name=admin'#&password=1234456` now this will let you log in without checking the password                  **if the username exist**                                                         


# **Sequel**
what i learned from it , is:
1. MySQL server on p 3306
2. when need to log-in , first `-h` if you use remote server , second `-u` for username and 
   `-p` for password 
3. some commmand you will use
  ```MySQL
  SHOW databases;           : Prints out the databases we can access.
  USE {database_name};      : Set to use the database named {database_name}.
  SHOW tables;              : Prints out the available tables inside the current
database.
SELECT * FROM {table_name}; : Prints out all the data from the table       {table_name}.

 note *  here mean everything.

```


# **Crocodile**
   what i learned from it , is:
   1.  Nmap scanning switch employs the use of default scripts during a scan is -sC
   2. most of this we already take it in [[Tier0 "AIOF"]] check it

#  **Responder**
what i learned from it , is:
1. 