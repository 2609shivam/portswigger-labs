# CSRF - where token is tied to non-session cookie

## 📌 Lab Details
- **Title**: CSRF where token is tied to non-session cookie
- **Difficulty**: Practitioner
- **Category**: Cross-site Request Forgery
- **Lab URL**: https://portswigger.net/academy/labs/launch/e32727c0220c06992583ba03b02655e4edbc8c9127fff08a29ad3c40c169495d?referrer=%2fweb-security%2fcsrf%2fbypassing-token-validation%2flab-token-tied-to-non-session-cookie

## 🔍 Summary
This lab demonstrates a scenario where a **CSRF token is enforced**, but the **token is tied to a seperate** `csrfKey` **cookie**, which is **not properly bound to the session**. An attacker can **inject this cookie into the victim's browser via a reflected search function**, making the CSRF protection ineffective.

## 🛠 Steps to Solve
1. Log in using the given account.
2. Submit the email change form and capture the request in Burp.
3. Send the request to Burp Repeater and observe that changing the `session` cookie logs you out, but changing the `csrfKey`     cookie merely results in the CSRF token being rejected.
4. Intercept the request in the lab's **search** bar to observe that the `search term` gets reflected in the `Set-cookie`        header.
5. Perform a `header` injection in the **search** bar: `/?search=test%0d%0aSet-Cookie:%20csrfKey=YOUR-KEY%3b%20SameSite=None`.
6. Craft a **CSRF PoC** using the `csrfKey` and `token`.
7. Upload it to the Exploit Server, test it, and deliver to victim.

## 📖 Key Takeaways
- The `csrfKey` is **not bound to session or user identity**, making it injectable.
- A reflected input in a header (like `Set-Cookie`) can be used to **inject cookies into victim’s browser**.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/a8d232bb-9a69-46e5-82f8-6873a28597e0)
![image](https://github.com/user-attachments/assets/1f9ed97f-b6b2-4424-b26b-9e37af004b87)
![image](https://github.com/user-attachments/assets/652723e4-0c99-4af6-8222-937e1dc5f02e)
![image](https://github.com/user-attachments/assets/cdc4f459-f1bd-4a17-9fdc-1cefa4555978)
![image](https://github.com/user-attachments/assets/d757b820-6748-4f57-acde-3694f725a00d)
![image](https://github.com/user-attachments/assets/e8ea05e5-09aa-4bad-afb9-df8b9aba5fa0)
![image](https://github.com/user-attachments/assets/5c0043bd-5118-4881-9458-f3cc17bee445)
