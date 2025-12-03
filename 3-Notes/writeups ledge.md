# WRITEUP 

## LEDGER — DAY 1

1. Advanced Bug Bounty Recon: A Methodology That Uncovers Hidden Vulnerabilities
    
2. I Automated My Recon and Found More Critical Bugs
    
3. Master Recon P-1: 15+ Ways of Subdomain Scraping
    
4. How to Find IDORs Like a Pro
    
5. IDOR — My First P1 Writeup (InfosecWriteups)
    
6. IDOR: Deleting Comments Like a Boss!
    
7. The Lazy Hacker’s Guide to $500 Information Disclosure
    
8. Information Disclosure That Made Me $2000 in Under 5 Minutes
    
9. Information Disclosure Vulnerability Writeup (HackerOne Style)
    
10. File Upload Vulnerabilities — Practical Tests & Notes
    
11. I Studied 100+ SSRF Reports — Here’s What I Learned
    
12. $100–$1000 Subdomain Takeover — Methodology


## LEDGER — DAY 2 (updated)

1. How I Hijacked 100+ Accounts with Just a URL Change (IDOR → XSS chain). 
    
2. OAuth Misconfiguration Lead To 1-Click Account Takeover (ATO).
    
3. $600 Simple MFA Bypass — GraphQL (auth/authorization bypass
    
4. Day 23: The Polyglot Poison — Resume Upload → Remote Shell (file upload).
    
5. Blind SQL Injection in APIs and Real-World Scenario.
    
6. Top Vulnerabilities — Race Condition Examples (coupon/gift card abuse). 
    
7. How I Exploited a JWT Misconfiguration for Account Takeover & Admin Access.
    
8. Business Logic Flaw — Bypass Payment / Get Paid Features for Free.
9. Misconfigured Reset Password → Account Takeover (token leakage/poisoning). 
    
10. From Simple File Import to Full Server Exposure — SSRF. 
    
11. CORS Misconfiguration — The Silent Gateway to Cross-Origin Data Leaks. 
    
12. Unrestricted API Endpoint → Dumping All User Data (exposed API).

---
day 3
## LEDGER — Extended

13. How I Uncovered IDOR, XSS, and Full Account Takeover in a Single Hunt  
14. Insecure Direct Object References (IDOR) Hands-on Lab (Medium)  
15. SQL Injection and IDOR explained + 8 interview questions (MeetCyber, Oct 2025)  
16. Injection Vulnerabilities – Detailed overview (OWASP / Web App Security guide)  
17. What is IDOR Vulnerability? — Real examples from banking apps (WebAsha blog, 2025)  
18. Web App Security breakdown: XSS / Injection / Access Control (security guide)  
19. GitHub “bug-bounty-writeups” repository — curated public writeup list  
20. SagarSajeev blog — mixed XSS, IDOR, file-upload, logic bug writeups  
21. “IDOR – a modern age SQLi” article (enciphers.com) — conceptual view on auth flaws  
22. Writeup-DB site — assorted real bug bounty writeups (SQLi, IDOR, Auth bypass etc.)
# WRITEUP LEDGER — DAY 3 (what I gave you now)

1. CVE-2025-63416 — Stored XSS → Privilege Escalation (SelfBest chat) — Rohit (Nov 2, 2025.)
    
2. Reflected XSS in Citrix NetScaler (RelayState SSO) — SecPod (Nov 13, 2025). 
    
3. IBM Lakehouse (watsonx.data) — Stored XSS (CVE-2025-36139) — IBM security bulletin (Sep 18, 2025). [IBM](https://www.ibm.com/support/pages/node/7245387?utm_source=chatgpt.com)
    
4. Business logic: bypassing payment / unlimited discount — Spacelift case (Oct 9, 2025). [InfoSec Write-ups](https://infosecwriteups.com/business-logic-error-bypassing-payment-with-test-cards-77c6e3c36f16?utm_source=chatgpt.com)
    
5. JWT misconfiguration → account takeover / admin access — InfosecWriteups (Aug/Sep 2025). [InfoSec Write-ups](https://infosecwriteups.com/how-i-exploited-a-jwt-misconfiguration-for-account-takeover-and-admin-access-in-5-minutes-c2974899f4ec?utm_source=chatgpt.com)
    
6. OAuth misconfiguration pre-ATO examples — public HackerOne reports. [HackerOne](https://hackerone.com/reports/1074047?utm_source=chatgpt.com)
    
7. SSRF → internal metadata / high-impact chain — medium-style bounty writeup. [Medium](https://medium.com/%40theindiannetwork/how-i-exploited-an-ssrf-vulnerability-earned-5000-real-world-exploit-e8ded56ef9ce?utm_source=chatgpt.com)
    
8. Race condition / coupon abuse → infinite discount — InfosecWriteups / Medium case. [InfoSec Write-ups](https://infosecwriteups.com/bug-bounty-race-exploiting-race-conditions-for-infinite-discounts-a2cb2f233804?utm_source=chatgpt.com)
    
9. IDOR real-world bounty case study (exfil / takeover) — Medium / HackerOne style writeup. [Medium](https://medium.com/%40beingnile/day-39-idor-report-7f0b3e82a7d5?utm_source=chatgpt.com)
    
10. File upload → RCE / web shell chain (real researcher writeup). [Medium](https://medium.com/%40akashoffsec/how-i-found-rce-remote-code-execution-via-file-upload-7541816e5fc4?utm_source=chatgpt.com)
    
11. Blind XSS / OOB bounty examples (HackerOne public reports). [HackerOne](https://hackerone.com/reports/1091118?utm_source=chatgpt.com)
    
12. XSS/CSP cheat sheet + CSP bypass examples (PortSwigger / up-to-date vectors) — useful companion reading. 

**Writeups added:**

1. Mobile API IDOR → ATO
    
2. CSP misconfig → stored XSS
    
3. GraphQL introspection chain
    
4. Payment race-condition
    
5. ImageMagick SSRF
    

**HackerOne reports added:**  
6. #241008 Shopify Stored XSS  
7. #826361 GitLab SSRF  
8. #1027822 Starbucks FIle Upload → RCE  
9. #2054184 Null-byte upload → RCE  
10. #228377 Discourse SSRF  
11. #1074047 OAuth → ATO  
12. #847210 Slack file exec  
13. #875028 Uber parameter pollution auth bypass