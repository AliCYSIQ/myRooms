[[HTB-LABS]] [[Week4]]

# **Archetype**
this what i learn:
1. you can enter `smb` server by `smbclient` , and you can use those flages: `-N` mean **no password** . and can use `-L` to **List service or directory**
2. after we do this , there's some dir show and one of those is `backups` and to enter it
   we can use this command `smbclient -N \\\\{IP}\\backups` by this we will get shell and can use `dir` command to list what in there , then use `get` to download
3. when use `dir` you will find find file its name is `prod.dtsConfig`  and use get to download this file , then if you cat this file you will see code , in this code you will see password `M3g4c0rp123` . this is the code 
```
   <DTSConfiguration>
    <DTSConfigurationHeading>
        <DTSConfigurationFileInfo GeneratedBy="..." GeneratedFromPackageName="..." GeneratedFromPackageID="..." GeneratedDate="20.1.2019 10:01:34"/>
    </DTSConfigurationHeading>
    <Configuration ConfiguredType="Property" Path="\Package.Connections[Destination].Properties[ConnectionString]" ValueType="String">
        <ConfiguredValue>Data Source=.;Password=M3g4c0rp123;User ID=ARCHETYPE\sql_svc;Initial Catalog=Catalog;Provider=SQLNCLI10.1;Persist Security Info=True;Auto Translate=False;</ConfiguredValue>
    </Configuration>

```