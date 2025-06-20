# CSRF - vulnerability with no defenses

## 📌 Lab Details
- **Title**: CSRF vulnerability with no defenses
- **Difficulty**: Apprentice
- **Category**: Cross-site Request Forgery
- **Lab URL**: https://portswigger.net/web-security/learning-paths/csrf/csrf-how-to-construct-a-csrf-attack/csrf/lab-no-defenses#

## 🔍 Summary
This PortSwigger lab demonstrates a classic **CSRF** (Cross-Site Request Forgery) vulnerability in the **email change functionality** of a web application.

## 🛠 Steps to Solve
1. Log into the given account.
2. Intercept the update email request.
3. Generate a **CSRF PoC** using Burp Suite for the given request.
4. Go to the Exploit server and store the generated **CSRF PoC**.
5. Deliver Exploit to the victim to complete the lab.

## 📖 Key Takeaways
- **Auto-submitting HTML** forms can perform state-changing actions without user interaction.
- **CSRF is possible when:**
  - There is no CSRF token.
  - The endpoint accepts cross-origin POSTs silently.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/7921b45a-a813-4828-a32b-127ef08e0947)
![image](https://github.com/user-attachments/assets/10eb8d1b-c15e-45c4-9ddb-c6f085006183)
![image](https://github.com/user-attachments/assets/5479d665-d2b2-4952-b702-7f8f2d77418f)
![image](https://github.com/user-attachments/assets/94012b3b-f672-4e93-a1b7-762ff4456b5b)
