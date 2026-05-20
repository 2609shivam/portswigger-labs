#  Blind OS command injection with time delays

## 📌 Lab Details
- **Title**: Blind OS command injection with time delays
- **Difficulty**: Practitioner
- **Category**: OS Command Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/28bda028c1ba840a8f4fccaaad3739fdc8d8e2d195f348c0db992cbcd6971cbf?referrer=%2fweb-security%2fos-command-injection%2flab-blind-time-delays

## 🔍 Summary
This lab demonstrates a **blind OS command injection** vulnerability in the feedback functionality of the web application.
The application executes a system command using user-supplied input from the feedback form. Although the command output is not visible in the HTTP response, it is still possible to confirm command execution using **time delays**.

## 🛠 Steps to Solve
1. Use Burp Suite to intercept and modify the request that submits feedback.
2. Modify the `email` parameter, changing it to:
   ```sh
   email=x & sleep 10 #
   ```
3. URL encode the value and send the request to solve the lab.

## 📖 Key Takeaways
- **Blind OS command injection** occurs when command output is not visible, but attackers can still confirm execution indirectly.
- Characters such as `||`, `&&`, and `;` can be used to chain malicious commands.
- **Time-based payloads** are effective for detecting blind command injection vulnerabilities.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/593800dc-58ca-45b7-acb7-6c900e54b6d3" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cee73bd0-7665-41e0-a1a1-82cee470afba" />
