# XSS - Reflected XSS into a JavaScript string with angle brackets HTML encoded

## 📌 Lab Details
- **Title**: Reflected XSS into a JavaScript string with angle brackets HTML encoded
- **Difficulty**: Apprentice  
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/164e6fe824ef894ffa05160602259af63f6266c2e82f19714daf79a0be75e363?referrer=%2fweb-security%2fcross-site-scripting%2fcontexts%2flab-javascript-string-angle-brackets-html-encoded

## 🔍 Summary
This lab demonstrates a reflected `DOM-based XSS` vulnerability where user input is inserted inside a JavaScript string. Angle brackets are HTML encoded, but JavaScript string injection is possible.

## 🛠 Steps to Solve
1. Submit a random alphanumeric string into the search box and intercept the request using Burp Suite.
2. Observe that the random string has been reflected inside a JavaScript string.
3. Injecting the payload in the search box: `'-alert(1)-'`.
4. This triggers an alert which verifies the attack.
   
## 📖 Key Takeaways
- XSS can occur `inside Javascript code`, not just HTML.
- HTML encoding angle brackets doesn't prevent JS string injection.
- Payload like `'-alert(1)-'` closes the string and runs JS code.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/b6815bdf-3614-4e79-bc04-72f5d09b9b4e)
![image](https://github.com/user-attachments/assets/f332fe31-592d-47b0-9c94-9b966748c185)
![image](https://github.com/user-attachments/assets/20aa527d-d822-4b4b-b0a1-510c8134302e)
![image](https://github.com/user-attachments/assets/89155320-69d8-4192-818f-1dd06d8c75b4)
![image](https://github.com/user-attachments/assets/7fbf0c40-95bc-4940-97d6-06407a5f3df3)
