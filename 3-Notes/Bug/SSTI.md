[[Bug]]
Server-side template injection is when an attacker is able to use native template syntax to inject a malicious payload into a template, which is then executed server-side.

in certain circumstances it could lead to **`RCE`**, even if you can't do `RCE` it could lead potentially gaining read access to sensitive data and arbitrary files on the server 


## Constructing a server-side template injection attack
to create a successful attack, you need to follow these setups :

![[Pasted image 20251025162851.png]]

### Detect

 try fuzzing by injecting a sequence of special characters commonly used in template expressions, such as **`${{<%[%'"}}%\`** If an exception is raised, This is one sign that a vulnerability to **`SSTI`** may exist.

**`SSTI`** occur in two distinct contexts, each of which requires its own detection method.

if fuzzing was inconclusive, a vulnerability may still reveal itself using one of these approaches. Even if fuzzing did suggest a template injection vulnerability, you still need to identify its context in order to exploit it.

