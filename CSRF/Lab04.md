# CSRF - where token is not tied to user session

## 📌 Lab Details
- **Title**: CSRF where token is not tied to user session
- **Difficulty**: Practitioner
- **Category**: Cross-site Request Forgery
- **Lab URL**: https://portswigger.net/academy/labs/launch/9eae9022063a6e38c28fc4b7904b4ac75ca324323ceb51dac385ad7dc5c734e5?referrer=%2fweb-security%2fcsrf%2fbypassing-token-validation%2flab-token-not-tied-to-user-session

## 🔍 Summary
This lab demonstrated a **CSRF vulnerability** where CSRF token is not tied to user session.

## 🛠 Steps to Solve
1. Log in as `wiener:peter`, submit the **email change form**, and **intercept the request** in Burp.
2. Note down the **CSRF** token value (from the hidden input).
3. Drop the request.
4. In a **private window**, log in as `carlos:montoya`.
5. In Burp Repeater, send a CSRF-protected request using the token **from the Wiener account**.
6. Observe: the request still succeeds.
7. Craft an **HTML PoC** using the token you grabbed and a new email.
8. Host it on the **Exploit Server**, test it, and **deliver to victim**.

## 📖 Key Takeaways
- Real tokens should be **session-specific** and **user-bound**.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/d8b28f80-b0c7-47b0-b24a-6f1eb33531ea)
![image](https://github.com/user-attachments/assets/12561a8d-76df-4dc2-8994-c79370d6785c)
![image](https://github.com/user-attachments/assets/40658741-ea51-4e65-a772-0423166cf20b)
![image](https://github.com/user-attachments/assets/02b50c09-b87a-456b-86b4-afe393b7251c)
![image](https://github.com/user-attachments/assets/7c3f5f68-f5cc-4d42-a8ed-7f95ce35b581)
![image](https://github.com/user-attachments/assets/9525c091-44bc-40a6-a9a8-c285e877dead)
![image](https://github.com/user-attachments/assets/fa2ea242-867e-4f48-bd0f-e55003933f93)
