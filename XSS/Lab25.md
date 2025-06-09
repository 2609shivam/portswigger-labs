# XSS - Reflected XSS with AngularJS

## 📌 Lab Details
- **Title**: Reflected XSS with AngularJS sandbox escape without strings
- **Difficulty**: Expert
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/d82e3a0ca07b096a36c320bdaf6f10ac92869a49f98d1adfd16b4ffdc05390f1?referrer=%2fweb-security%2fcross-site-scripting%2fcontexts%2fclient-side-template-injection%2flab-angular-sandbox-escape-without-strings

## 🔍 Summary
This lab contains a **reflected XSS** vulnerability where `eval` function cannot be used to insert string into **Angular JS**.

## 🛠 Steps to Solve
1. `toString().constructor`
   - `toString()` gives you a string.
   - `.constructor` gives you the **String constructor**.
     
2. `prototype.charAt=[].join`
   - 

## 📖 Key Takeaways
- AngularJS can be exploited even when strings and `$eval` are blocked.
- Bypasses use prototype pollution (`charAt`) and string building (`fromCharCode`).
  

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/dcdb95ae-3762-43ed-bde2-61c5552159a9)
![image](https://github.com/user-attachments/assets/e9e73c1d-84c8-47c1-9859-4621714a6b53)
![image](https://github.com/user-attachments/assets/3bc2f854-42b5-4596-b2a6-ecbf3cbfed68)
