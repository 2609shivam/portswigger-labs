#  Multi-step process with no access control on one step

## 📌 Lab Details
- **Title**: Multi-step process with no access control on one step
- **Difficulty**: Practitioner
- **Category**: Access-Control
- **Lab URL**: https://portswigger.net/academy/labs/launch/48ea5eff7577d0d5e50c64b5d91f3e2fcc175ebb055c95f30bf8e909f6ba9d2c?referrer=%2fweb-security%2faccess-control%2flab-multi-step-process-with-no-access-control-on-one-step

## 🔍 Summary
The admin panel used a multi-step role-change flow, but the **confirmation** step lacked authorization. Capturing the confirmation request as an admin and replaying it with a non-admin session cookie allowed `wiener:peter` to promote themself.

## 🛠 Steps to Solve
1. Log in using the admin credentials.
2. Browser to the admin panel, promote **carlos**, send the confirmation HTTP request to Burp Repeater.
3. Open a private/incognito browser window, and log in with the non-admin credentials.
4. Copy the non-admin user's session cookie into the existing Repeater request, change the username to yours, and replay it.

## 📖 Key Takeaways
- Enforce auth on every step of multi-step workflows (not just the first).
- Bind confirmations to the initiating session or use single-use signed tokens.
- Protect state-changing endpoints with CSRF and anti-replay measures.
- Log and alert on role-change/confirmation requests.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4689d76d-4a54-42ad-8519-7a62ded54f73" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f7f108a8-514f-492f-881e-2dc41da8e0fb" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/450e8f1f-1cd2-4b31-a38a-a26cd82dfba2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/72842160-b6b9-44bf-b57c-379e5e1b003e" />
