# CSRF - where Referer validation depends on header being present

## 📌 Lab Details
- **Title**: CSRF where Referer validation depends on header being present
- **Difficulty**: Practitioner
- **Category**: Cross-site Request Forgery
- **Lab URL**: https://portswigger.net/academy/labs/launch/827802fa22cc24000bb63cd4a2787ba5076b3c9727323ea512dc392ba98842b8?referrer=%2fweb-security%2fcsrf%2fbypassing-referer-based-defenses%2flab-referer-validation-depends-on-header-being-present

## 🔍 Summary
This lab demonstrates bypassing **Referer-based CSRF** protection by exploiting the server’s insecure fallback behavior:
If the Referer header is absent, the server accepts the request, allowing **CSRF**.

## 🛠 Steps to Solve
1. Log in to the given account.
2. Send the `change email` request to **Burp Repeater** and observe that if you change the **header domain** the request is rejected.
3. Delete the **Referer header** entirely and observe that the request is now accepted.
4. Generate a CSRF PoC and include the following `HTML` in the `head` tag: `<meta name="referrer" content="no-referrer">`.
5. Store the exploit and deliver to the victim to solve the lab.

## 📖 Key Takeaways
- **Referer-based CSRF** protection is weak if the server accepts requests when the header is missing.
-  Always use proper **CSRF tokens** instead of relying solely on **Referer** checks.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/a45c37ed-b616-4b05-ac9b-4fe9c28090ed)
![image](https://github.com/user-attachments/assets/a81ea09e-2c63-4a08-b339-aae237d83a2e)
![image](https://github.com/user-attachments/assets/c5dbe3d0-c8da-4f17-bd99-0102ced9b7c4)
![image](https://github.com/user-attachments/assets/abca681e-735c-42bf-9e71-0ce406540a8a)
![image](https://github.com/user-attachments/assets/3394bf93-fb4f-436a-a543-115bc621923a)
![image](https://github.com/user-attachments/assets/cb0cf77e-a62c-40d1-b404-41ff91f523ce)
