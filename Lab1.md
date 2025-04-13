# XSS - Reflected XSS into HTML context 

## 📌 Lab Details
- **Title**: Reflected XSS into HTML context with nothing encoded
- **Difficulty**: Apprentice
- **Category**: Cross Site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/9e3414ee887ebaed9f7ed25b58dd639959bb28a3b926f53c831c1d86e03d8813?referrer=%2fweb-security%2fcross-site-scripting%2freflected%2flab-html-context-nothing-encoded

## 🔍 Summary
This lab demonstrated a simple reflected cross-site vulnerability in the `search` functionality.

## 🛠 Steps to Solve
1. Identified the vulnerable parameter: `?search`.
2. Injected the following payload: `<script>alert(1)</script>`.
3. Click `Search`.
   
## 📖 Key Takeaways
- Reflected XSS occurs when user input is immediately included in the server's response.
- If input is inserted directly into HTML without encoding, it opens the door for arbitrary script execution.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/04949819-c3db-4428-826f-64a0ae2c05f5)
![image](https://github.com/user-attachments/assets/004e636c-228a-4bae-a375-8ccb7cad66c6)
![image](https://github.com/user-attachments/assets/68070d74-4dd7-4f8a-bb17-89b23eebdd75)

