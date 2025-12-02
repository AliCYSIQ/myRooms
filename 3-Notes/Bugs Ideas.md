[[Bug]]

### check password rest


### OAuth Misconfiguration 

Missing validation of the OAuth `state` parameter allowed me to link an attacker-controlled OAuth identity to an existing victim account. By sending a single crafted link (one click), an attacker can cause a victim’s account to become linked to the attacker’s OAuth identity, enabling account takeover via the OAuth login button. This is an authentication / account-linking logic flaw and results in full account takeover if the attacker controls the linked identity


able to create account with victime email(without verfiy it ) then let the victim log in using Oauth , here the attacker will be able to log in using the password he set
### idor 

check uuid sometimes it  work without need cookie or anything


able to use the test card to purchase the premium subscription on the website. I used card number as 4242 4242 4242 4242, date as 10/23 and a random cvv number 333 to purchase the premium and I was successfully able to purchase it
Many websites now a days has resolved this issue by implementing a check against test cards provided by [https://stripe.com/docs/testing#cards](https://stripe.com/docs/testing#cards) but I have figured out a small bypass for those websites who has limit this fix. We can still use the test cards but this technique will be applied to few websites only. Instead of using the random numbers or the cards provided by [https://stripe.com/docs/testing#cards](https://stripe.com/docs/testing#cards), you can pick the test cards from [https://www.fakenamegenerator.com/](https://www.fakenamegenerator.com/) and can check whether it works or not.