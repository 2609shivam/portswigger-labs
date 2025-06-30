# CSRF - with broken Referer validation

## 📌 Lab Details
- **Title**: CSRF with broken Referer validation
- **Difficulty**: Practitioner
- **Category**: Cross-site Request Forgery
- **Lab URL**: https://portswigger.net/academy/labs/launch/16d017d6cc0162f41ea71950a84e47e8a8b7914d65e26f917ea8fd2366287791?referrer=%2fweb-security%2fcsrf%2fbypassing-referer-based-defenses%2flab-referer-validation-broken

## 🔍 Summary
This lab shows how **Referer-based CSRF protection can be bypassed** if the server only checks if its domain **appears anywhere in the Referer header**.

## 🛠 Steps to Solve
1. Log in to the given account.
2. Send the `change email` request to **Burp Repeater** and observe that if you change the **header domain** the request is rejected.
3. Copy the original domain of your lab instance and append it to the Referer header in the form of a query string. Send the request and observe that it is now accepted.
4. Generate a **CSRF PoC** and edit the JavaScript for the third argument: `history.pushState("", "", "/?YOUR-LAB-ID.web-security-academy.net")`.
5. For full **URL** to be included in the request, add the following header to the **"Head"** section: `Referre-Policy: unsafe-url`.

## 📖 Key Takeaways
-  Weak Referer checks that match substrings can be bypassed by appending the target domain in a query string.
-  Shows why relying on **Referer checks** for **CSRF protection** is unsafe; **CSRF tokens** should be used.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/f7f88c12-e62c-4c0f-a91f-710895212962)
![image](https://github.com/user-attachments/assets/9f5965a9-8baf-49f7-9b9f-dc4c4a4f31ee)
![image](https://github.com/user-attachments/assets/33a7a57e-0839-4a15-bc87-f117a7dde962)
![image](https://github.com/user-attachments/assets/70d4a64f-e78c-4456-96a1-6fd1a4b0cbd2)
