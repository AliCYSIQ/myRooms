[[Bug]]
Server-side template injection is when an attacker is able to use native template syntax to inject a malicious payload into a template, which is then executed server-side.

in certain circumstances it could lead to **`RCE`**, even if you can't do `RCE` it could lead potentially gaining read access to sensitive data and arbitrary files on the server 