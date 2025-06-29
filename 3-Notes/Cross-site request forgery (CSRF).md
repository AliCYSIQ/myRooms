[[Portswigger]]

it vulnerability that allows the attacker to induce users to perform actions that they do not intend to perform, example:
![[ccrfEMAIL.svg]]
### **the impact of CSRF** 
======================
the attacker causes the victim user to carry out an action unintentionally, like let him change its email or password or even transmit funds to another account that the attacker 
have and if this attack apply on an admin of the website it maybe change a lot of things that led to destroy the  whole website by delete users account or share their passwords and emails.

This meets the conditions required for CSRF:

- The action of changing the email address on a user's account is of interest to an attacker. Following this action, the attacker will typically be able to trigger a password reset and take full control of the user's account.
- The application uses a session cookie to identify which user issued the request. There are no other tokens or mechanisms in place to track user sessions.
- The attacker can easily determine the values of the request parameters that are needed to perform the action.
(complete later)