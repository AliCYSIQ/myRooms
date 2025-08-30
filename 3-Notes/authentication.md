[[Bug]]
Authentication vulnerabilities can allow attackers to gain access to sensitive data and functionality. They also expose additional attack surface for further exploits.

## What is authentication?

Authentication is the process of verifying the identity of a user or client. Websites are potentially exposed to anyone who is connected to the internet. This makes robust authentication mechanisms integral to effective web security.

There are three main types of authentication:

- Something you **know**, such as a password or the answer to a security question. These are sometimes called "knowledge factors".
- Something you **have**, This is a physical object such as a mobile phone or security token. These are sometimes called "possession factors".
- Something you **are** or do. For example, your biometrics or patterns of behavior. These are sometimes called "inherence factors".

## What is the difference between authentication and authorization?

Authentication is the process of verifying that a user is who they claim to be. Authorization involves verifying whether a user is allowed to do something.
For example, authentication determines whether someone attempting to access a website with the username `Carlos123` really is the same person who created the account.

Once `Carlos123` is authenticated, their permissions determine what they are authorized to do. For example, they may be authorized to access personal information about other users, or perform actions such as deleting another user's account.

## Vulnerabilities arise?

Most vulnerabilities in authentication mechanisms occur in one of two ways:

- The authentication mechanisms are weak because they fail to adequately protect against brute-force attacks.
- Logic flaws or poor coding in the implementation allow the authentication mechanisms to be bypassed entirely by an attacker. This is sometimes called "broken authentication".

In many areas of web development, logic flaws cause the website to behave unexpectedly, which may or may not be a security issue. However, as authentication is so critical to security, it's very likely that flawed authentication logic exposes the website to security issues.

### Username enumeration

Username enumeration is when an attacker is able to observe changes in the website's behavior in order to identify whether a given username is valid.

Username enumeration typically occurs either on the login page, for example, when you enter a valid username but an incorrect password, or on registration forms when you enter a username that is already taken. This greatly reduces the time and effort required to brute-force a login because the attacker is able to quickly generate a shortlist of valid usernames.

While attempting to brute-force a login page, you should pay particular attention to any differences in:

- **Status codes**: During a brute-force attack, the returned HTTP status code is likely to be the same for the vast majority of guesses because most of them will be wrong. If a guess returns a different status code, this is a strong indication that the username was correct. It is best practice for websites to always return the same status code regardless of the outcome, but this practice is not always followed.
- **Error messages**: Sometimes the returned error message is different depending on whether both the username AND password are incorrect or only the password was incorrect. It is best practice for websites to use identical, generic messages in both cases, but small typing errors sometimes creep in. Just one character out of place makes the two messages distinct, even in cases where the character is not visible on the rendered page.
- **Response times**: If most of the requests were handled with a similar response time, any that deviate from this suggest that something different was happening behind the scenes. This is another indication that the guessed username might be correct. For example, a website might only check whether the password is correct if the username is valid. This extra step might cause a slight increase in the response time. This may be subtle, but an attacker can make this delay more obvious by entering an excessively long password that the website takes noticeably longer to handle.

## Vulnerabilities in password-based login
### Brute-force attacks

A brute-force attack is when an attacker uses a system of trial and error to guess valid user credentials. These attacks are typically automated using word lists of usernames and passwords. Automating this process, especially using dedicated tools, potentially enables an attacker to make vast numbers of login attempts at high speed.

Brute-forcing is not always just a case of making completely random guesses at usernames and passwords. By also using basic logic or publicly available knowledge, attackers can fine-tune brute-force attacks to make much more educated guesses. This considerably increases the efficiency of such attacks. Websites that rely on password-based login as their sole method of authenticating users can be highly vulnerable if they do not implement sufficient brute-force pro

**some labs** sometimes brute-forcing have really big logic flaw which lead to account take over
like some tell you if the username is the wrong or the password , which is big mistake from the website like if the username is wrong tell you `username is wrong` and if the password was wrong will tell you `password is wrong`

but some of them fix this by telling you every time `username or password is wrong.`
you can try to input **true username** and see what will happened and when i do this , this is what happened `usernamme or password is wrong` it the same but not it is not , there is no `.` in the end which mean it different response and this is logic flew lead to brute force 

and some of them depend of the time of the response 
`X-Forwarded-For` let you spoof your ip in website and bypass **ip-block-protection**
now for different time , every time the username is **invalid** the response time will be the same (or close to be the same) but if it valid it will be different depend on the password length so you can let the password like 100-length and enumerate he username then see what the different in time response or the most length one , mostly it will be the valid username, then you can enumerate the passwords  and use `X-Forwarded-For` to **bypass internet address block**
[need to complete from here](https://portswigger.net/web-security/authentication/password-based#Flawed%20brute-force%20protection)

some time if you try to sign in 3 times and all of them are fails the website will block you for some minute  but if you try to sign in 2 times and fails then sign in for the 3 time was correct the counter of attempt will reset which will be logic flaw because you can do 2 random sign in (to get the victim account) then sign in with your account and by this you can make brute force 

sometimes every work good and as it should be , but if you try to login the request that have the password in `json` so you cant change it from `"password": "wowmy"` 
to `"password": ["p1","p2",...]` which will try to use all the passwords in one request


## Vulnerabilities in multi-factor authentication
Many websites rely exclusively on single-factor authentication using a password to authenticate users. However, some require users to prove their identity using multiple authentication factors.

first vulnerability you may face is you can bypass 2 factor authentication by just drop the request of  2FA , for example you sign in after that it ask for 2FA you can just drop the request (using burp) then go to your account page and it will let you in , or you can just when arriving to the OTP step just enter `website.com/dashboard` instead of the 2FA URL
___
some times the website do not verify if you were the same user or no in 2FA for example
```
POST HTTP/2 
Set-Cookie: verify=username; ssession:...
```
you can change it to 
```
POST HTTP/2 
Set-Cookie: verify=victime-username; ssession:...
```
and if it let try without limit , then this is full account takeover 
____

sometimes when you try to enter 2FA code it will let you try just two or three times then it will let you out and this is the only way the website use as protection against Burt force which is vulnerable
all you need to use is macros (i will write about macros later)
_______
sometimes all you need to do it is just let blank , for example in 2FA holder you write `1111` then using `burp` remove it and let it blank(nothing written) and send the request and it will be passed and you will be login
_____
 good i dont know how to explain it but this is the [link](https://hackerone.com/reports/2885636)
this bug let you use another user email(in condition that email do not have account) and to do that you need to active 2FA then go and change your email to the victim email here the website will not ask for OTP to make sure if you have this email or not and if the emaail owner try to make account they will see " this email have account" 
_____
sometimes OTP will be generate every some time (e.g. 30s) and if the new one is generated , the old should be unusable  but sometimes it still be able to reuse it which lead to brutforce which is bug
_____
bug  i dont know how to explain it but this is the [link](https://hackerone.com/reports/1050244?utm_source=chatgpt.com)
_____
you can bypass 2FA only be delete one cookie like `bb_refresh` and the website will not ask for 2FA

**Target (redacted):** `sso.target.com`

🔍 Vulnerability Flow

1. **Login flow observed:**
    
    - `POST /v2/login` (username submitted)
        
    - Redirects → `GET /v2/login/options` (asks for password)
        
    - Wrong password → `POST /v2/login` → server response: `302` → `GET /v2/login?failed`
        
2. **Exploit:**
    
    - Replace failed request with:
      POST /v2/mfalogin/enrolled

    Forward the request → application grants access.

Root cause:

    Application assigned a valid session cookie after username entry.

    /v2/mfalogin/enrolled endpoint did not validate authentication state or MFA.

source : [link](https://medium.com/%40sharp488/critical-account-takeover-mfa-auth-bypass-due-to-cookie-misconfiguration-3ca7d1672f9d)
_____
you can bypass 2FA by removing `CSRF` token and it will send you to page with 
`invaild csrf tocken`  all you need here just change the URL to any other page like `setteings`
you can see it [here](https://youtu.be/NJyN7Ys7BC4)

_____

**you can** bypass login by `forget password` which will send rest password link to your email , you can intercept the  request and try to add your email next to the victime email(there's such way to do that and i will add most of then here)


---

# Authentication Vulnerabilities

Authentication vulnerabilities can allow attackers to gain access to sensitive data and functionality. They also expose additional attack surface for further exploits.

---

## What is Authentication?

Authentication is the process of verifying the identity of a user or client. Since websites are potentially exposed to anyone on the internet, robust authentication mechanisms are integral to web security.

### Types of authentication:

- **Something you know** → e.g., password, security question (knowledge factors).
    
- **Something you have** → e.g., phone, hardware token (possession factors).
    
- **Something you are / do** → e.g., biometrics, behaviour patterns (inherence factors).
    

---

## Authentication vs Authorisation

- **Authentication** = verifying that the user is who they claim to be.
    
- **Authorisation** = verifying what the user is allowed to do.
    

Example: `Carlos123` authenticates with correct password → authenticated.  
But whether Carlos can delete other users depends on **authorisation**.

---

# Vulnerabilities in Authentication

Vulnerabilities usually come from:

- Weak mechanisms that don’t protect against brute-force.
    
- Logic flaws or poor implementation that allow bypassing authentication entirely (_broken authentication_).
    

---

## Vulnerabilities in Password-Based Login

### 1. User Enumeration

- Some websites disclose whether _username_ or _password_ is incorrect.
    
- Example: error messages differ:
    
    - Wrong username → “username is wrong”
        
    - Wrong password → “password is wrong”
        
- Fix attempt: combine into generic: “username or password is wrong.”
    
- But even here, differences in punctuation (e.g. one has a full stop, one doesn’t) can leak validity → **logic flaw → brute force possible**.
    

### 2. Timing Attacks

- Response time differences (valid vs invalid usernames) can allow attackers to infer accounts.
    

### 3. Brute-Force Protection Flaws

- Some sites block after N attempts.
    
- Logic flaw: if you try 2 wrong passwords, then 1 correct (on your own account), the counter resets.
    
- Attacker can keep brute-forcing 2 attempts, then reset with a login to their own account → unlimited attempts.
    

---

## Vulnerabilities in Multi-Factor Authentication (MFA)

Many sites use MFA, but flawed implementations make it bypassable.  
Grouped by type:

---

### A) Flow Bypass

- **Dropping the request**: After login, the server asks for 2FA. If you drop that request in Burp and directly load `/dashboard`, access is granted.
    
- **Direct URL access**: Instead of submitting OTP, navigate straight to `/dashboard`.
    

---

### B) Improper State / Binding

- **User verification cookie manipulation**:
    
    ```
    Set-Cookie: verify=username; session=...
    ```
    
    Change to:
    
    ```
    Set-Cookie: verify=victim-username; session=...
    ```
    
    → If no rate limits, full account takeover.
    
- **Session token set pre-MFA**: Some apps assign a valid session cookie _after username entry_, before OTP validation.  
    Example exploit flow:
    
    ```
    POST /v2/login → session created
    POST /v2/mfalogin/enrolled → app grants access without MFA
    ```
    
    Source: [Medium write-up](https://medium.com/@sharp488/critical-account-takeover-mfa-auth-bypass-due-to-cookie-misconfiguration-3ca7d1672f9d).
    
- **Email change without OTP verification**:  
    HackerOne [report #2885636](https://hackerone.com/reports/2885636).  
    User enables 2FA → changes email to victim’s. Site doesn’t verify ownership. Victim can’t register (email “already taken”).
    

---

### C) Rate-Limiting Flaws

- Some sites allow only 2–3 OTP attempts, then block.
    
- Attacker can automate with Burp Macros to cycle through → brute-force OTPs.
    
- Old OTPs still valid even after new ones are generated → reuse flaw → brute-force window extended.
    

---

### D) Input Validation Failures

- Sending blank OTP (e.g., enter `1111` but strip it in Burp to nothing) → login granted.
    
- OTP reuse across sessions (H1 [report #2529780](https://hackerone.com/reports/2529780)).
    

---

### E) CSRF / Token Handling

- Remove CSRF token in OTP submission → server returns “invalid CSRF token.”
    
- Change URL manually to `/settings` → bypass.
    
- Demonstrated in [YouTube video](https://youtu.be/NJyN7Ys7BC4).
    

---

### F) Cookie Manipulation

- Delete a specific cookie (`bb_refresh`) → site no longer asks for 2FA.
    
- Full access without OTP prompt.
    

---

### G) Reported MFA Logic Flaws

- **Account recovery flaw**: H1 [report #1050244](https://hackerone.com/reports/1050244). 2FA enforcement bypass during recovery flow.
    
- **Snapchat OTP flaw**: H1 [report #921780](https://hackerone.com/reports/921780). Login/logout flow mishandled OTP → account takeover.
    

---

# Summary

Authentication and MFA bypass flaws often come from:

1. **Weak error handling** (enumeration, timing).
    
2. **Improper flow/state binding** (session before OTP, cookie values, recovery missteps).
    
3. **Poor rate-limiting** (OTP brute force, reset logic).
    
4. **Validation issues** (blank input, OTP reuse).
    
5. **Session/cookie mishandling** (delete or alter one cookie → bypass).
    

These flaws almost always lead to **account takeover despite MFA**, which is critical impact.
