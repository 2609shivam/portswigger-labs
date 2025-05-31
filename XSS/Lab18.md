# XSS - Reflected XSS into a JavaScript string

## 📌 Lab Details
- **Title**: Reflected XSS into a JavaScript string with single quote and backslash escaped
- **Difficulty**: Practitioner
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/d926f2eb65b5882b27710e31d931b82bff5eccf019671ce0d921d6d23fd97442?referrer=%2fweb-security%2fcross-site-scripting%2fcontexts%2flab-javascript-string-single-quote-backslash-escaped

## 🔍 Summary
This lab has a **reflected DOM-based XSS** vulnerability inside a Javascript string. Your input is reflected into a `<script>` block, and characters like single quotes `'` and backslashes `\` are escaped. You need to break out this context and inject your own `<script>` tag to execute `alert(1)`.

## 🛠 Steps to Solve
1. Submit a random alphanumeric string into the search box.
2. Use Burp Suite to observe that the input appears inside a `Javascript` string.
3. Try entering `test'payload` and see how it's escaped.
4. Inject the payload: `</script><script>alert(1)</script>`.

## 📖 Key Takeaways
- Escaped quotes (`\'`) prevent simple string-breaking payloads.
- When quotes are escaped, you can **inject a new script tag** to bypass restrictions.
- Reflected XSS inside JavaScript requires understanding the **exact code context**.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/bb011dad-98d2-4b9b-bfab-13b476456d1b)
![image](https://github.com/user-attachments/assets/864cba54-b039-4e1c-8f7e-1a2f485bf863)
![image](https://github.com/user-attachments/assets/d971c7f3-784c-4fe7-b7c3-953bf7fa2eae)
![image](https://github.com/user-attachments/assets/54ea035b-8ad0-47f2-a9fd-4ba060008408)
![image](https://github.com/user-attachments/assets/14510400-37ab-47cf-aac5-89bbf637a06d)
![image](https://github.com/user-attachments/assets/1c2a4c70-b8f2-4328-b94b-8a1bebbf7a87)
