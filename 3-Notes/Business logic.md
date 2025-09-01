[[Bug]]

## What are business logic vulnerabilities?

Business logic vulnerabilities are flaws in the design and implementation of an application that allow an attacker to elicit unintended behavior. This potentially enables attackers to manipulate legitimate functionality to achieve a malicious goal. These flaws are generally the result of failing to anticipate unusual application states that may occur and, consequently, failing to handle them safely.

## How do business logic vulnerabilities arise?

Business logic vulnerabilities often arise because the design and development teams make flawed assumptions about how users will interact with the application. These bad assumptions can lead to inadequate validation of user input. For example, if the developers assume that users will pass data exclusively via a web browser, the application may rely entirely on weak client-side controls to validate input. These are easily bypassed by an attacker using an intercepting proxy.

## Excessive trust in client-side controls

A fundamentally flawed assumption is that users will only interact with the application via the provided web interface. This is especially dangerous because it leads to the further assumption that client-side validation will prevent users from supplying malicious input. However, an attacker can simply use tools such as Burp Proxy to tamper with the data after it has been sent by the browser but before it is passed into the server-side logic. This effectively renders the client-side controls useless.
