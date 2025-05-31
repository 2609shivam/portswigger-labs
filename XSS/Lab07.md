# XSS - Reflected XSS into attribute with angle brackets HTML-encoded

## 📌 Lab Details
- **Title**: Reflected XSS into attribute with angle brackets HTML-encoded
- **Difficulty**: Apprentice
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/8db2d4d5d579849ca64098e4415c35888c0af17e8f9b0849462456d7b14b8087?referrer=%2fweb-security%2fcross-site-scripting%2fcontexts%2flab-attribute-angle-brackets-html-encoded

## 🔍 Summary
This lab demonstrates a reflected XSS vulnerability where user input is injected into a quoted HTML attribute. By breaking out of the attribute and injecting an `onmouseover` event, we trigger JavaScript execution.

## 🛠 Steps to Solve
1. Identified the vulnerable parameter: `search blog`.
2. Submitted a random alphanumeric string to observe the vulnerable `attribute`.
3. Injecting the payload `"onmouseover="alert(1)` into the `search blog`.
4. Verified the technique by copying the URL to a different window and hovering the mouse over the search blog.
   
## 📖 Key Takeaways
- Even when angle brackets are encoded, XSS is possible via attribute injection.
- Using `"onmouseover="alert(1)` breaks the attribute context and injects a new one.
- Event-based XSS can trigger JavaScript without needing `<script>` tags.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/fc04c5f0-3d00-4391-aae9-ce2ef544dbcc)
![image](https://github.com/user-attachments/assets/30d8c59c-f640-4456-b74c-d514ae94b963)
![image](https://github.com/user-attachments/assets/cc8b8cf9-6314-4a8e-b6f7-2e2d940f0d0e)
![image](https://github.com/user-attachments/assets/baefbf6b-df51-4dd2-a105-e2d7d8430579)
