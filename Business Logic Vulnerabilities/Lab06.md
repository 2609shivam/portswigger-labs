#  Authentication Bypass

## 📌 Lab Details
- **Title**: Authentication Bypass via flawed state machine
- **Difficulty**: Practitioner
- **Category**: Business Logic
- **Lab URL**: https://portswigger.net/academy/labs/launch/f009e13156910cf7897bc7ed85b32662e8b2753196347950c3b3c51d86b9c3c5?referrer=%2fweb-security%2flogic-flaws%2fexamples%2flab-logic-flaws-authentication-bypass-via-flawed-state-machine

## 🔍 Summary
This lab demonstrates a **flawed state machine** in the authentication process. The application assumes users will follow the intended login sequence, including role selection.
By interrupting this flow, it is possible to **bypass role assignment** and gain **administrator access**, leading to full compromise of the application.

## 🛠 Steps to Solve
1. Login with `wiener:peter`.
2. Observe login flow in Burp:
   - After `POST /login`, server redirects to:
     ```sh
     GET /role-selector
     ```
3. Discover **admin panel**: `GET /admin`.
4. Accessing `/admin` normally fails (no admin role).
5. **Exploit the state machine**
   - Enable **Burp Intercept**
   - Login again
   - Forward:
     ```sh
     POST /login
     ```
   - Intercept:
     ```sh
     GET /role-selector
     ```
   - **Drop this request**
6. Access **home page**.
7. Result: **Role defaults to **administrator**.
8. Delete user `carlos`.

## 📖 Key Takeaways
- Authentication flows must enforce **strict state transistions**.
- Skipping intermediate steps can lead to **privilege escalation**.
- Never rely on frontend-driven workflows for security.
- Always validate roles and states on the server side.

## 🖼️ Screenshot 
<img width="1919" height="1012" alt="image" src="https://github.com/user-attachments/assets/931375a8-15a4-44bd-81d7-4b46769b6a5d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/11dfe160-464c-4ade-873e-ec8e0c2b549f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3ce389fb-add9-45a6-805c-5adf124b7248" />
<img width="1919" height="1021" alt="image" src="https://github.com/user-attachments/assets/ab832c71-a7e9-491a-b5d4-3d505de311a1" />
<img width="1919" height="1018" alt="image" src="https://github.com/user-attachments/assets/c8a58cfe-d481-4803-b665-f30d3a8fd55b" />
