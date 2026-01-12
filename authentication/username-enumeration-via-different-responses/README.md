# Lab: Username enumeration via different responses
This write-up documents the analysis and exploitation of the authentication
vulnerability demonstrated in the PortSwigger Web Security Academy lab  
**“Username enumeration via different responses”**.


**Lab Link:** [Username enumeration via different responses](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses)
---

## Lab Objective

The objective of this lab is to identify a valid username by analyzing differences
in the application's responses to failed login attempts and then successfully
log in to the application.

---

## Vulnerability Overview

**Username enumeration** occurs when an application reveals whether a username
exists based on observable differences in authentication responses.

Such differences may include:
- Error messages
- Response content
- HTTP status codes
- Behavioral differences

In this lab, the vulnerability is caused by **different error messages** returned
when an invalid username is supplied compared to when a valid username with an
incorrect password is used.

---

## Analysis

During the authentication process, the application responds differently depending
on the supplied username:

- **Invalid username** → the server returns a specific error message
- **Valid username + incorrect password** → a different error message is returned

By sending multiple authentication requests with various usernames and a fixed
incorrect password, these differences can be observed and used to reliably
enumerate valid usernames.

This behavior directly leaks information about existing user accounts.

---

## Evidence (Screenshots)

The following screenshots demonstrate the difference in server responses during
authentication attempts.

**Response for an invalid username:**

![Invalid username response](screenshots/username_attack.png)

**Response for a valid username with an incorrect password:**

![Valid username response](screenshots/password_attack.png)

---

## Proof of Concept / Lab Reproduction Steps

The steps below describe how the vulnerability can be reproduced in a controlled
lab environment.

1. Intercept the login request using a web proxy tool (e.g. **Burp Suite**)
2. Send multiple authentication attempts with different usernames
3. Keep the password value constant and incorrect
4. Observe and compare the server responses
5. Identify the username that produces a distinct response
6. Use the identified valid username to successfully complete the lab

---

## Impact

Username enumeration significantly weakens authentication mechanisms.

An attacker can:
- Identify valid user accounts
- Reduce the search space for brute-force attacks
- Increase the likelihood of account compromise

---

## Mitigation

To prevent username enumeration vulnerabilities:

- Use **generic error messages** for all authentication failures
- Ensure consistent response content and HTTP status codes
- Implement **rate limiting** and account lockout mechanisms
- Avoid disclosing whether a username exists during authentication

---

## References

- PortSwigger Web Security Academy  
- OWASP Top 10 – Identification and Authentication Failures

---

## Author

**cybertanja**
