# CSRF - where token validation depends on token being present

## 📌 Lab Details
- **Title**: CSRF where token validation depends on token being present
- **Difficulty**: Practitioner
- **Category**: Cross-site Request Forgery
- **Lab URL**: https://portswigger.net/academy/labs/launch/464b3ce68cb4a94b23c4dca52f5535b394cb9d940479664818f3fd06c514cd42?referrer=%2fweb-security%2fcsrf%2fbypassing-token-validation%2flab-token-validation-depends-on-token-being-present

## 🔍 Summary
This lab demonstrates a **CSRF vulnerability** where CSRF token validation is only enforced if the token is present.

## 🛠 Steps to Solve
1. Log in using the given account.
2. Submit the email change form and capture the request.
3. Send to **Repeater** and observe that changing `csrf` token causes rejection.
4. Remove the `csrf` parameter and observe that the request is now accepted.
5. Craft an **CSRF PoC** with the new email.
6. Upload it to the Exploit Server, test it, and deliver to victim.

## 📖 Key Takeaways
- **CSRF** tokens must be validated whether they are present or not.
- Proper responses should be set by the developer if an **CSRF** token is not found.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/6aaac373-4350-46a4-9933-52506cc9faff)
![image](https://github.com/user-attachments/assets/009574ba-2a11-4cd0-a57f-25ad1b071672)
![image](https://github.com/user-attachments/assets/9968024d-8d07-4a6d-8849-9bee28b133da)
![image](https://github.com/user-attachments/assets/c3b872f2-9bef-44e9-aef8-f9f45615eb5c)
![image](https://github.com/user-attachments/assets/839cd1e3-a121-4ec6-a97e-3a6b0682bf48)
![image](https://github.com/user-attachments/assets/ef99c6b5-fe17-4ea6-a362-43a32e552c62)
