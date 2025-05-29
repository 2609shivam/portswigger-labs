# XSS - Reflected DOM XSS

## 📌 Lab Details
- **Title**: Reflected DOM XSS
- **Difficulty**: Practitioner
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/954173de38d3d0f6588c4a98d6400a1335a3ef78d9a8ce701a6609fb87941e96?referrer=%2fweb-security%2fcross-site-scripting%2fdom-based%2flab-dom-xss-reflected

## 🔍 Summary
This lab has a **DOM-based reflected XSS vulnerability** where user input is reflected into a **Javascript context** using `eval()`. The input is not properly sanitized, allowing an attacker to inject and execute malicious Javascript.

## 🛠 Steps to Solve
1. Search for a test string like `XSS` using the search bar on the site.
2. **Intercept the request** in Burp Suite. You'll see input is reflected in a JSON string (e.g., `"search:XSS"`).
3. View the `searchResults.js` file in the browser’s dev tools or Burp Site Map — it uses `eval()` on the search input.
4. Exploit the unsafe eval() : `\"-alert(1)}//`.
   
## 📖 Key Takeaways
- Using `eval` on reflected user input is very dangerous.
- Escaping characters like quotes is **not enough** if other characters (like `\') are still allowed.
- **Backslash - based escape** is a common XSS bypass.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/d25d9221-7a97-47b1-9611-b3e36a261b46)
![image](https://github.com/user-attachments/assets/7da8d615-c5a7-4890-ac51-dfddff9a4098)
![image](https://github.com/user-attachments/assets/d3f556b7-d38f-43d3-aeb7-391dda2a1abb)
![image](https://github.com/user-attachments/assets/e54823f2-e2e6-43f3-b98f-192215b3f0a0)
![image](https://github.com/user-attachments/assets/a15aba90-8a1e-4e13-a32d-04907d474671)
![image](https://github.com/user-attachments/assets/d04b0aef-3e52-409c-86aa-188b889225fe)
