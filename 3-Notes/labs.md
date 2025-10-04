Short answer: no — the first list was a good start but **not** exhaustive for real-world bug-bounty prep. Below I’m giving you a **practical, exhaustive (for everyday bug-bounty training) catalog** split exactly as you asked: **Online (browser-accessible)** and **Self-hosted (run locally / VM / Docker)**. Each item is 1–3 lines so you can copy it. I prioritized the best, most-used targets that cover OWASP Top-10, APIs, auth/logic, SSRF, client-side, GraphQL, and scanner testbeds.

I’ll cite the most important sources up front. ([OWASP Foundation](https://owasp.org/www-project-vulnerable-web-applications-directory/?utm_source=chatgpt.com "OWASP Vulnerable Web Applications Directory"))

---

# Online (hosted — open in browser)

1. **Gin & Juice Shop — `https://ginandjuice.shop/`**  
    PortSwigger’s public demo — modern e-commerce testbed for scanners and manual testing (XSS, SQLi, logic, auth). Great for quick browser practice. ([ginandjuice.shop](https://ginandjuice.shop/?utm_source=chatgpt.com "Gin & Juice Shop: Home"))
    
2. **PortSwigger — Web Security Academy (labs)**  
    Interactive, guided labs (including API labs) covering the whole methodology you need for bug bounties — free and trackable. Ideal for learning Burp techniques and real proof-of-concepts. ([PortSwigger](https://portswigger.net/blog/gin-and-juice-shop-put-your-scanner-to-the-test?utm_source=chatgpt.com "Gin and Juice Shop: put your scanner to the test | Blog"))
    
3. **Google Firing Range (public)** — `https://public-firing-range.appspot.com/`  
    Scanner/testbed that contains many edge-case XSS/CSRF/SSRF variants; useful to test scanners and find subtle detection gaps. ([public-firing-range.appspot.com](https://public-firing-range.appspot.com/?utm_source=chatgpt.com "Firing Range"))
    
4. **Google XSS Game / XSS Challenge** — `https://xss-game.appspot.com/` (or xssgame.com)  
    Short, focused XSS puzzles that teach DOM vs reflected vs stored XSS in a browser. Fast drills. ([xss-game.appspot.com](https://xss-game.appspot.com/?utm_source=chatgpt.com "XSS game - Google App Engine"))
    
5. **Google Gruyere** — small codelab with guided exercises (path traversal, XSS, CSRF, RCE) — great tutorial-style practice. ([google-gruyere.appspot.com](https://google-gruyere.appspot.com/?utm_source=chatgpt.com "Web Application Exploits and Defenses"))
    
6. **PentestGround (online disposable targets)**  
    Temporary vulnerable targets (various apps/APIs) that reset — handy for testing tools and practice without installing anything.
    
7. **HackTheBox (HTB) — Web labs & Pro labs**  
    Paid+free lab platform with real web machines and realistic scopes; good for chaining web → host pivots once you’re comfortable. (Account required.)
    
8. **TryHackMe — Web / API rooms**  
    Guided rooms that include web + API challenges; good if you like structured learning paths (free + paid). (Account required.)
    
9. **PentesterLab (online exercises)**  
    High-quality web vuln exercises (some free, much paid). Great for stepwise learning and writeups for reports.
    
10. **Root-Me / HackThisSite**  
    Community CTF sites with web challenges, logic flaws, and practice tasks — useful to sharpen creative exploit thinking.
    

---

# Self-hosted (run locally — Docker / VM / LAMP / Java)

1. **OWASP Juice Shop** — `docker run --rm -p 3000:3000 bkimminich/juice-shop`  
    Modern single-page app with _tons_ of real flaws and challenges (OWASP Top-10 + chained issues). Run locally for safe, repeatable practice. ([GitHub](https://github.com/vavkamil/awesome-vulnerable-apps?utm_source=chatgpt.com "vavkamil/awesome-vulnerable-apps"))
    
2. **Damn Vulnerable Web App (DVWA)** — GitHub `digininja/DVWA`  
    Classic PHP/MySQL app with levelled difficultly (SQLi, XSS, CSRF, file upload). Good for basics and Burp practise. ([GitHub](https://github.com/digininja/DVWA?utm_source=chatgpt.com "digininja/DVWA: Damn Vulnerable Web Application (DVWA)"))
    
3. **bWAPP (bee-box VM available)** — `https://www.itsecgames.com/`  
    100+ bugs in PHP; includes lessons on file upload, RCE, and many OWASP Top-10 items. Bee-box VM makes setup trivial. ([itsecgames.com](https://www.itsecgames.com/?utm_source=chatgpt.com "bWAPP, a buggy web application!"))
    
4. **OWASP WebGoat**  
    Java app with step-by-step lessons — excellent for server-side, auth, and logic flaw education.
    
5. **OWASP Mutillidae II**  
    LAMP-style vulnerable app with many hints — handy for classroom or CTF prep. ([OWASP Foundation](https://owasp.org/www-project-mutillidae-ii/?utm_source=chatgpt.com "OWASP Mutillidae II"))
    
6. **Security Shepherd (OWASP)**  
    Training platform (web + mobile) designed to take you from novice → expert with gamified lessons. ([OWASP Foundation](https://owasp.org/www-project-security-shepherd/?utm_source=chatgpt.com "OWASP Security Shepherd"))
    
7. **DVNA / DVNA-forks (Damn Vulnerable NodeJS App)**  
    Node/Express targeted app to practice Node-specific vulns (misused eval, insecure deserialization, etc.). ([GitHub](https://github.com/appsecco/dvna?utm_source=chatgpt.com "appsecco/dvna: Damn Vulnerable NodeJS Application"))
    
8. **vAPI (Vulnerable Adversely Programmed Interface)** — `vapi.apisec.ai` (self-host)  
    Purpose-built **API** lab that mimics OWASP API Top-10 (BOLA, auth, rate limiting, parameter pollution). Must-have for modern bug bounties where APIs are the attack surface. ([vapi.apisec.ai](https://vapi.apisec.ai/?utm_source=chatgpt.com "vAPI"))
    
9. **SQLi-Labs / sqli-labs** (GitHub)  
    Focused SQL-injection exercises (error, blind, time, second-order). Great for mastering payloads and WAF bypasses. ([GitHub](https://github.com/Audi-1/sqli-labs?utm_source=chatgpt.com "SQLI labs to test error based, Blind boolean ..."))
    
10. **XVWA (Xtreme Vulnerable Web App)**  
    Old but dense PHP app covering SQLi, RCE, PHP Object Injection, SSRF, file inclusion — host locally only. ([GitHub](https://github.com/s4n7h0/xvwa?utm_source=chatgpt.com "XVWA is a badly coded web application written in PHP ..."))
    
11. **Vulnerable-GraphQL / vulnapi GraphQL**  
    Purpose-built GraphQL labs to practice ACL/IDOR/Query abuse on GraphQL endpoints.
    
12. **Hackazon (Rapid7)**  
    E-commerce site mimic; useful for API + frontend flows (some community mirrors exist). ([Medium](https://cyberrey.medium.com/sql-injection-sqli-hands-on-lab-d049af02b623?utm_source=chatgpt.com "SQL Injection (SQLi) Hands-on Lab | by Cyber Rey - Medium"))
    
13. **BadStore / BadStore.net**  
    Demo app that shows common issues in shopping workflows; useful for logic/flow testing.
    
14. **BodgeIt Store**  
    Beginner friendly vulnerable web app — quick to deploy for practice.
    
15. **DSVW (Damn Small Vulnerable Web)**  
    Compact vulnerable app for quick labs.
    
16. **Metasploitable2 / Metasploitable3 (VMs)**  
    Vulnerable VM images with multiple services and web apps — good for full-stack compromise practice. (Run only offline.)
    
17. **VulnHub (collection of downloadable VMs)**  
    Repository of deliberately vulnerable VMs — many include real web-app scenarios for chaining web → host escalations. (Use VirtualBox/VMware.)
    
18. **OWASP Broken Web Applications (BWA) VM**  
    A packaged VM that bundles many vulnerable apps (Mutillidae, etc.) — fast lab environment.
    
19. **Security Dojo / Web Security Dojo**  
    Prebuilt VM with a collection of vulnerable web apps and tools for training.
    
20. **SSTI / Template-injection playgrounds, JWT/OAuth playgrounds**  
    Small, single-purpose apps (often GitHub projects) to specifically practice SSTI, JWT misconfigurations, OAuth flows and SSO abuse.
    

---

# API / Modern attack surface (must-practice items)

- **vAPI** (mentioned above) — OWASP API Top-10 scenarios. ([vapi.apisec.ai](https://vapi.apisec.ai/?utm_source=chatgpt.com "vAPI"))
    
- **APIsec / APISec University labs** — courses and hands-on API scenarios (useful for platform-level API armor). ([apisecuniversity.com](https://www.apisecuniversity.com/?utm_source=chatgpt.com "APISec University: Free API Security Training for AppSec ..."))
    
- **GraphQL vulnerable labs** — search “vulnerable-graphql-api” on GitHub (practice GraphQL introspection, field abuse, BOLA). ([GitHub](https://github.com/arainho/awesome-api-security?utm_source=chatgpt.com "A collection of awesome API Security tools and resources."))
    

---

# One-line practical plan (copyable)

1. Learn technique: **Web Security Academy** labs (PortSwigger). ([PortSwigger](https://portswigger.net/blog/gin-and-juice-shop-put-your-scanner-to-the-test?utm_source=chatgpt.com "Gin and Juice Shop: put your scanner to the test | Blog"))
    
2. Modern app practice: run **OWASP Juice Shop** locally. ([GitHub](https://github.com/vavkamil/awesome-vulnerable-apps?utm_source=chatgpt.com "vavkamil/awesome-vulnerable-apps"))
    
3. Classic exploit practice: run **DVWA** + **SQLi-Labs** locally. ([GitHub](https://github.com/digininja/DVWA?utm_source=chatgpt.com "digininja/DVWA: Damn Vulnerable Web Application (DVWA)"))
    
4. API mastery: deploy **vAPI** and practice BOLA/IDOR. ([vapi.apisec.ai](https://vapi.apisec.ai/?utm_source=chatgpt.com "vAPI"))
    
5. Scanner edge cases: test against **Gin & Juice Shop** and **Firing Range**. ([ginandjuice.shop](https://ginandjuice.shop/?utm_source=chatgpt.com "Gin & Juice Shop: Home"))
    

---

### Final blunt note (practical mentor speak)

This list covers **all major categories** you’ll hit in bug-bounty work: classic injection/XSS, modern SPAs, APIs/GraphQL, auth/logic, SSRF/SSO, and scanner edge cases. It’s not fan-fiction — it’s the actual toolbox everyone uses. If you want, I’ll now:

• produce a **single `docker-compose.yml`** that brings up Juice Shop + DVWA + vAPI + WebGoat on different ports (copy/paste ready), **or**  
• give you a **7-day practice plan** that uses only online targets (Gin&Juice + Web Security Academy + Firing Range) and self-hosted ones in parallel.

Which one do you want me to drop in next? (I’ll just produce it — no waiting.)