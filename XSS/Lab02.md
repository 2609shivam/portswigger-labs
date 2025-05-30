# XSS - Stored XSS into HTML context

## 📌 Lab Details
- **Title**: Stored XSS into HTML context with nothing encoded
- **Difficulty**: Apprentice
- **Category**: Cross-site scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/09e0bee59e32e92a78e614589793110f95c397459ed0ab96a3dbaf100e3b7cde?referrer=%2fweb-security%2fcross-site-scripting%2fstored%2flab-html-context-nothing-encoded

## 🔍 Summary
This lab demonstrated a stored cross-site scripting vulnerability in the `comment` functionality.

## 🛠 Steps to Solve
1. Identified the vulnerable parameter: `comment`.
2. Injected payload into the comment box: `<script>alert(1)</script>`.
3. Enter a name, email and a website.
4. Click "Post comment".
   
## 📖 Key Takeaways
- Stored XSS means the malicious script is permanently stored on the server, such as in a blog comment, post, or profile.
- In this lab, the comment section directly inserted user input into the HTML page without any encoding or sanitization.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/c9da5aca-247c-48d7-b2be-458e8f450e4a)
![image](https://github.com/user-attachments/assets/52280b1b-5c29-4791-be6a-8d112c4cb873)
![image](https://github.com/user-attachments/assets/8d57d22b-c98e-42ba-bcfd-b0501c154baf)
