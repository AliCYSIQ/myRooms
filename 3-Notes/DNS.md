[[Network]]

**DNS** (Domain Name System): translating human-readable domain names (like google.com) into numerical IP addresses that computers use to find each other online. It's essential for accessing websites, sending emails, and using other internet services. Without DNS, we'd have to remember complex IP addresses instead of simple domain names.
and we can call it **DNS Resolution**.

### **How this work?**

for example you try to get access to **app.hackthebox.com** to get this first you will see if the browser have cash this domain if not , it will see your OS and if it don't have it, it will need to ask you DNS Provider and if it dont have it and it dont cach it , it will ask **The Roots** servers which they are 13 one and the server will ask about the top-domain for example here

![[URL _Parts.png]]

so it will ask about which server have `.com` top-level domain and it will answer with the server that have this TLD(top level domain) then will ask this server about the SLD(second-level domain) then this server will answer with the name of the server that have this domain 

___
