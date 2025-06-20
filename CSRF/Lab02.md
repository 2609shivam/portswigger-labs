# CSRF - where token validation depends on request method

## 📌 Lab Details
- **Title**: CSRF where token validation depends on request method
- **Difficulty**: Practitioner
- **Category**: Cross-site Request Forgery
- **Lab URL**: https://portswigger.net/web-security/learning-paths/csrf/csrf-common-flaws-in-csrf-token-validation/csrf/bypassing-token-validation/lab-token-validation-depends-on-request-method#

## 🔍 Summary
This lab demonstrates a **CSRF vulnerability** where CSRF token validation is only enforced for **POST** requests, not for GET requests.

## 🛠 Steps to Solve
1. Log in using the given account.
2. Submit the email change form and capture the request in Burp.
3. Send to Repeater and observe that changing the **csrf** token causes rejection.
4. Change the **method** to **GET** — notice that CSRF token is no longer required.
5. Craft an **CSRF PoC** using a **GET** request with the email parameter.
6. Upload it to the Exploit Server, test it, and deliver to victim.

## 📖 Key Takeaways
- **CSRF** tokens must be validated for all state-changing requests, not just **POST**.
- Allowing sensitive actions via **GET** enables CSRF without token.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/09ddfaef-6055-4125-afdb-1ff49707e0ef)
![image](https://github.com/user-attachments/assets/5e50fb02-f0a8-42ff-ace9-4e0596b2a880)
![image](https://github.com/user-attachments/assets/07a7df68-7e12-4f27-b19d-8b437ef1a5ce)
![image](https://github.com/user-attachments/assets/a51ac76d-7dce-4453-bb29-86a60d0fe61d)
![image](https://github.com/user-attachments/assets/e9f35f31-8539-4e8c-9469-44c5b627805c)
![image](https://github.com/user-attachments/assets/9fe1472f-e88c-4c5f-b137-82422f1b34fa)
