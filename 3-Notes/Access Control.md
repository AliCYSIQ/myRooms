[[Bug]]

## What is access control?

**Access control** is the application of constraints on who or what is authorized to perform actions or access resources. In the context of web applications, access control is dependent on authentication and session management:

- **Authentication** confirms that the user is who they say they are.
- **Session management** identifies which subsequent HTTP requests are being made by that same user.
- **Access control** determines whether the user is allowed to carry out the action that they are attempting to perform.

### Vertical access controls

Vertical access controls are mechanisms that restrict access to sensitive functionality to specific types of users.
### Horizontal access controls

Horizontal access controls are mechanisms that restrict access to resources to specific users.

### Context-dependent access controls

Context-dependent access controls restrict access to functionality and resources based upon the state of the application or the user's interaction with it.
Context-dependent access controls prevent a user performing actions in the wrong order. For example, a retail website might prevent users from modifying the contents of their shopping cart after they have made payment.

----

### Vertical privilege escalation

If a user can gain access to functionality that they are not permitted to access then this is vertical privilege escalation. For example, if a non-administrative user can gain access to an admin page where they can delete user accounts, then this is vertical privilege escalation.

#### changing URL

you can change the url it self to go to  a page you should not get it , like `website.com/admin`
sometimes the link be unguessable  

#### Parameter-based access control methods

Some applications determine the user's access rights or role at login, and then store this information in a user-controllable location. This could be:

- A hidden field.
- A cookie.
- A preset query string parameter.

most of it just focus on the response or cookie 
#### Broken access control resulting from platform misconfiguration
some website might configure rules like this one:

`DENY: POST, /admin/deleteUser, managers`

This rule denies access to the `POST` method on the URL `/admin/deleteUser`, for users in the managers group. Various things can go wrong in this situation, leading to access control bypasses.
it will deny it if `/admin/deleteUser` in the URL  but some of them let you do this 
```
POST / HTTP/1.1 
X-Original-URL: /admin/deleteUser
```
there's no `admin` in the URL so it will passed but the header here will let you perform `/admin/deleteUser`

you need to see [portswigger access control](https://portswigger.net/web-security/access-control)



### Horizontal privilege escalation
Horizontal privilege escalation occurs if a user is able to gain access to resources belonging to another user, instead of their own resources of that type. For example, if an employee can access the records of other employees as well as their own, then this is horizontal privilege escalation.

sometimes every user have its unique `id` and if this id was easy to guess like `?id=1` then this is a bug 
other times it be hard to guess like (GU-IDs) This may prevent an attacker from guessing or predicting another user's identifier. However, the GU-IDs belonging to other users might be disclosed elsewhere in the application where users are referenced, such as user messages or reviews.

in some application even if you have this `id` the application detect you here you need to see the response it will have the cookies of the users and you can use it to log-in 

### Access control vulnerabilities in multi-step processes

Many websites implement important functions over a series of steps. This is common when:

- A variety of inputs or options need to be captured.
- The user needs to review and confirm details before the action is performed.

\
For example, the administrative function to update user details might involve the following steps:

1. Load the form that contains details for a specific user.
2. Submit the changes.
3. Review the changes and confirm.

Sometimes, a website will implement rigorous access controls over some of these steps, but ignore others. Imagine a website where access controls are correctly applied to the first and second steps, but not to the third step. The website assumes that a user will only reach step 3 if they have already completed the first steps, which are properly controlled. An attacker can gain unauthorized access to the function by skipping the first two steps and directly submitting the request for the third step with the required parameters.

### Referer-based access control

Some websites base access controls on the `Referer` header submitted in the HTTP request. The `Referer` header can be added to requests by browsers to indicate which page initiated a request.

for example if there's `/admin` and the website will send `403` if you try to access it , try go sub-page like `webiste/admin/deletUser` and to do that you need to change `Referer` header



### Location-based access control

Some websites enforce access controls based on the user's geographical location. This can apply, for example, to banking applications or media services where state legislation or business restrictions apply. These access controls can often be circumvented by the use of web proxies, VPNs, or manipulation of client-side geolocation mechanisms.


## IDOR
## What are insecure direct object references (IDOR)?

Insecure direct object references (IDOR) are a type of access control vulnerability that arises when an application uses user-supplied input to access objects directly. The term IDOR was popularized by its appearance in the OWASP 2007 Top Ten. However, it is just one example of many access control implementation mistakes that can lead to access controls being circumvented. IDOR vulnerabilities are most commonly associated with horizontal privilege escalation, but they can also arise in relation to vertical privilege escalation.

mostly depeaand on logic flaw and miscofinguration

## reports
I pulled **actual HackerOne / Bugcrowd disclosures** and **public write-ups** (Medium, InfoSecWriteups, CybersecurityWriteups). Below are **15+** items: **10 platform disclosures** and **≥5 public write-ups**. I include a one-line impact note for each and the source so you can cite .

---

# Final answer — curated list (copy-paste friendly)

## A) Platform disclosures (HackerOne / Bugcrowd) — **10** real reports

1. **HackerOne — Report #2487889 — “Insecure Direct Object Reference”** — IDOR exposing private report metadata; platform disclosure page. ([HackerOne](https://hackerone.com/reports/2487889?utm_source=chatgpt.com "HackerOne | Report #2487889 - Insecure Direct Object Reference ..."))
    
2. **Bugcrowd — Unauthorized Team & Organization Creation via IDOR (Globe app)** — lack of userId validation in POST allowed unauthorized object creation. ([Bugcrowd](https://bugcrowd.com/disclosures/d8ed3e0d-923b-49a8-8a46-767d81fcd5c6/unauthorized-team-and-organization-creation-via-insecure-direct-object-reference-idor?utm_source=chatgpt.com "Unauthorized Team and Organization Creation via Insecure Direct ..."))
    
3. **HackerOne — Report #262661 — IDOR on Feedback/Review functionality** — create or manipulate public reviews via IDOR. ([HackerOne](https://hackerone.com/reports/262661?utm_source=chatgpt.com "Report #262661 - IDOR on HackerOne Feedback Review"))
    
4. **HackerOne — Report #46397 — Insecure Direct Object Reference vulnerability** — example disclosure showing impact on participant/notification flows. ([HackerOne](https://hackerone.com/reports/46397?utm_source=chatgpt.com "Report #46397 - Insecure Direct Object Reference vulnerability"))
    
5. **HackerOne — Report #120115 (Veris)** — critical IDOR enabling actions like deleting organisation members (high impact). ([HackerOne](https://hackerone.com/reports/120115?utm_source=chatgpt.com "Veris | Report #120115 - Critical - Insecure Direct Object Reference"))
    
6. **HackerOne — Report #751577 — IDOR allowed access to payments data of any user** — exposes financial information via object ID manipulation. ([HackerOne](https://hackerone.com/reports/751577?utm_source=chatgpt.com "Report #751577 - IDOR allow access to payments data of any user"))
    
7. **HackerOne — Report #358143 (Yelp)** — critical IDOR allowing manipulation/association of objects leading to serious business logic abuse. ([HackerOne](https://hackerone.com/reports/358143?utm_source=chatgpt.com "Yelp | Report #358143 - CRITICAL Insecure Direct Object Reference ..."))
    
8. **HackerOne — Report #2633771 — IDOR on AddTagToAssets operation** — API object reference vulnerability in asset/tag operations. ([HackerOne](https://hackerone.com/reports/2633771?utm_source=chatgpt.com "IDOR Vulnerability at AddTagToAssets operation name | HackerOne"))
    
9. **HackerOne — Report #126861 — IDOR on badoo.com** — classic IDOR disclosure on a mainstream service. ([HackerOne](https://hackerone.com/reports/126861?utm_source=chatgpt.com "Report #126861 - Insecure Direct Object Reference on badoo.com"))
    
10. **HackerOne/Bugcrowd — Atlassian / community.atlassian.com IDOR disclosure** — remove users or manipulate groups via IDOR; Bugcrowd published the disclosure summary. ([Bugcrowd](https://bugcrowd.com/disclosures/2fce1eaa-b482-4279-a7c8-8ca89cdad472/idor-remove-users-from-community-groups?utm_source=chatgpt.com "IDOR - remove users from community groups - Bugcrowd"))
    

> Notes: platform pages often redact bounty amounts — the important part is the public disclosure and impact description. Examples above include high-impact findings (account takeover, exposure of payment data, organisation/member deletion).

---

## B) Public write-ups / blog posts (Medium, InfoSecWriteups, CybersecurityWriteups) — **≥5**

11. **InfoSecWriteups — “Hunting IDOR: A Deep Dive into Insecure Direct Object References”** — practical hunting tips + real examples. ([InfoSec Write-ups](https://infosecwriteups.com/%EF%B8%8F-hunting-idor-a-deep-dive-into-insecure-direct-object-references-b550a9f77333?utm_source=chatgpt.com "Hunting IDOR: A Deep Dive into Insecure Direct Object References"))
    
12. **InfoSecWriteups — “IDOR — my first P1 in Bug Bounty”** — personal writeup of finding & reporting a P1 IDOR; useful for PoC style. ([InfoSec Write-ups](https://infosecwriteups.com/idor-insecure-direct-object-references-my-first-p1-in-bugbounty-fb01f50e25df?utm_source=chatgpt.com "IDOR “Insecure direct object references”, my first P1 in Bugbounty"))
    
13. **CybersecurityWriteups — “Hacking My Way to $3,000: Unmasking a Sneaky IDOR”** — hands-on writeup with impact and payout described. ([Cyber Security Write-ups](https://cybersecuritywriteups.com/hacking-my-way-to-3-000-unmasking-a-sneaky-idor-vulnerability-%EF%B8%8F-%EF%B8%8F-06ebcb65ba9a?utm_source=chatgpt.com "Hacking My Way to $3000: Unmasking a Sneaky IDOR Vulnerability ..."))
    
14. **Medium — “All About Insecure Direct Object Reference (IDOR)” (Rohit Singh)** — solid primer + examples you can reference in notes. ([Medium](https://medium.com/%40insightfulrohit/all-about-insecure-direct-object-reference-idor-666cad6a94f0?utm_source=chatgpt.com "All About Insecure Direct Object Reference(IDOR) | by Rohit Singh"))
    
15. **Medium — “About IDOR (Insecure Direct Object Reference)” (Joko Purwanto)** — another clear explainer and hunting guide. ([Medium](https://medium.com/%40jokodfir/about-idor-insecure-direct-object-reference-929fa9e1b272?utm_source=chatgpt.com "About IDOR (Insecure Direct Object Reference) | by Joko Purwanto"))
    
16. **Medium — “Insecure Direct Object Reference (IDOR) Vulnerabilities” (technical overview)** — useful for mapping technique → impact ↔ remediation. ([Medium](https://medium.com/%40jetti.dinesh/insecure-direct-object-reference-idor-vulnerabilities-df551431eb7b?utm_source=chatgpt.com "Insecure Direct Object Reference (IDOR) Vulnerabilities - Medium"))
    
17. **InfoSecWriteups — “Critical IDOR vulnerability on Medium? — writeup/case study”** — case study style with concrete examples. ([InfoSec Write-ups](https://infosecwriteups.com/critical-idor-vulnerability-on-medium-f78346edbcb1?utm_source=chatgpt.com "Critical IDOR Vulnerability on Medium? - InfoSec Write-ups"))
    

---

## C) Extra practical resources (learning → reporting)

18. **Bugcrowd blog — “How-to: Find IDOR for large bounties”** — methodology for discovery & triage. ([Bugcrowd](https://www.bugcrowd.com/blog/how-to-find-idor-insecure-direct-object-reference-vulnerabilities-for-large-bounty-rewards/?utm_source=chatgpt.com "How-To: Find IDOR (Insecure Direct Object Reference ... - Bugcrowd"))
    
19. **PortsWigger Web Security Academy — IDOR lab** — canonical lab to practise object ID manipulation and access-control bypasses (great to pair with above write-ups). ([PortSwigger](https://portswigger.net/web-security/access-control/idor?utm_source=chatgpt.com "Insecure direct object references (IDOR) | Web Security Academy"))
    

---

### Quick guidance on using these in your report pack

- For each platform disclosure (A1–A10) paste the title + one-line impact (from the platform page) + link (use the platform page as evidence). I cited the platform pages above. ([HackerOne](https://hackerone.com/reports/2487889?utm_source=chatgpt.com "HackerOne | Report #2487889 - Insecure Direct Object Reference ..."), [Bugcrowd](https://bugcrowd.com/disclosures/d8ed3e0d-923b-49a8-8a46-767d81fcd5c6/unauthorized-team-and-organization-creation-via-insecure-direct-object-reference-idor?utm_source=chatgpt.com "Unauthorized Team and Organization Creation via Insecure Direct ..."))
    
- For each public write-up (B11–B17) include a short PoC excerpt and the bounty/payout if the author discloses it (some do — e.g., the $3,000 case in CybersecurityWriteups). ([Cyber Security Write-ups](https://cybersecuritywriteups.com/hacking-my-way-to-3-000-unmasking-a-sneaky-idor-vulnerability-%EF%B8%8F-%EF%B8%8F-06ebcb65ba9a?utm_source=chatgpt.com "Hacking My Way to $3000: Unmasking a Sneaky IDOR Vulnerability ..."))
    

---

