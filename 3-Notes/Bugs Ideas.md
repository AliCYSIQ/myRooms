[[Bug]]

### check password rest


### OAuth Misconfiguration 

Missing validation of the OAuth `state` parameter allowed me to link an attacker-controlled OAuth identity to an existing victim account. By sending a single crafted link (one click), an attacker can cause a victim’s account to become linked to the attacker’s OAuth identity, enabling account takeover via the OAuth login button. This is an authentication / account-linking logic flaw and results in full account takeover if the attacker controls the linked identity

### idor 

check uuid sometimes it  work without need cookie or anything 