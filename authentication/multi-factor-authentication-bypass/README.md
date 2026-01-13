# Write-up: 2FA simple bypass (PortSwigger Academy)

###  Objective
The goal of this lab is to bypass the two-factor authentication (2FA) mechanism and gain full access to the account of the user **carlos**.<br>
**Lab Link:** [2FA simple bypass](https://portswigger.net/web-security/authentication/multi-factor/lab-2fa-simple-bypass))

###  Tools
* **Browser:** Chrome/Firefox
* **Target:** PortSwigger Web Security Academy

---

###  Vulnerability Analysis
The application suffers from **broken authentication control**. It establishes a session after the first stage of authentication (username and password) but fails to properly restrict access to the account page if the second stage (2FA) is skipped. 

The server assumes that if a user reaches `/my-account`, they must have passed the 2FA, but it doesn't actually verify this state on the server side.



---

###  Step-by-Step Execution

#### 1. Analysis of the Normal Flow
1. Log in to your own account using `wiener:peter`.
2. Access the **Email client** to retrieve the 2FA code.
3. Submit the code and observe that you are redirected to `/my-account?id=wiener`.
4. **Log out** to clear the session.

#### 2. Authentication as the Victim
1. Go to the login page and enter the victim's credentials:
   * **Username:** `carlos`
   * **Password:** `montoya`
2. Click **Log in**. You are now redirected to the `/login2` page (the 2FA prompt).

#### 3. The Bypass
1. Instead of entering a code, go to the browser's address bar.
2. Manually change the URL from:
   `https://[LAB-ID].web-security-academy.net/login2`
   to:
   `https://[LAB-ID].web-security-academy.net/my-account?id=carlos`
3. Press **Enter**.

---

###  Result
The server grants access to Carlos's account page because it sees a valid session (started by the correct password) and fails to check if the 2FA step was completed. The lab banner will change to **"Lab Solved"**.

###  Remediation
To fix this, the application should:
* Implement a **"partial session"** state after the password check.
* Use a server-side flag (e.g., `is_2fa_complete: true`) that is only set after the correct code is entered.
* Redirect any requests to `/my-account` back to the 2FA page if this flag is not set.
