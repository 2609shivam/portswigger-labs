# XSS - Reflected XSS into a template literal

## 📌 Lab Details
- **Title**: Reflected XSS into a template literal with angle brackets, single, double quotes, backslash and backticks Unicode-escaped
- **Difficulty**: Practitioner
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/0a88d8a0c7685665c8baff00182659bf0a53f478d01725b703663bc726d81776?referrer=%2fweb-security%2fcross-site-scripting%2fcontexts%2flab-javascript-template-literal-angle-brackets-single-double-quotes-backslash-backticks-escaped

## 🔍 Summary
This lab has a **reflected XSS** vulnerability in a JavaScript **template literal**. Even though the app escapes dangerous characters like: `<`,`>`,`'`,`"`,`\` and backticks (`` ` ``). But it doesn't block `${...}` which allows **JavaScript** code execution.

## 🛠 Steps to Solve
1. Submit a random alphanumeric string into the search box and intercept the request using Burp Suite.
2. Observe that the random string has been reflected inside a **JavaScript template** string.
3. Inject the payload into the search box: `${alert(1)}`.
4. Use the URL to create an `alert`.

## 📖 Key Takeaways
- Template literals use backticks (` `) and support `${...}` for embedding JavaScript.
- Even if quotes and angle brackets are escaped, `${}` may still run code.
- This is a **JavaScript based XSS**, not an HTML one.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/d8264542-a08e-4afb-92ee-1d79ee4d22f7)
![image](https://github.com/user-attachments/assets/1bac8214-6200-4b38-8ad7-5d62bfa8b441)
![image](https://github.com/user-attachments/assets/6d2a06e0-09e3-4b07-9658-833ddc39ba02)
![image](https://github.com/user-attachments/assets/7ceedf97-816d-4752-b170-c41ad99e5e4d)
![image](https://github.com/user-attachments/assets/14c27171-c205-4b0f-b2a2-048aab462b6b)
