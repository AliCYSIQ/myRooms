[[HTB-LABS]] [[Week4]]

# **Archetype**
this what i learn:
1. you can enter `smb` server by `smbclient` , and you can use those flages: `-N` mean **no password** . and can use `-L` to **List service or directory**
2. after we do this , there's some dir show and one of those is `backups` and to enter it
   we can use this command `smbclient -N \\\\{IP}\\backups` by this we will get shell and can use `dir` command to list what in there , then use `get` to download
3. 