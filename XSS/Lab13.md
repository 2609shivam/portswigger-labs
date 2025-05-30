# XSS - Stored DOM 

## 📌 Lab Details
- **Title**: Stored DOM XSS
- **Difficulty**: Practitioner
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/a2e1b73b9b37946f3a6b2d3bef063907db04eede2ec15f0b9c17518818b81ec8?referrer=%2fweb-security%2fcross-site-scripting%2fdom-based%2flab-dom-xss-stored

## 🔍 Summary
This lab has a **Stored DOM-based XSS** vulnerability in the blog comment section. The website tries to block XSS by replacing only the **first** angle bracket (`<`), which is not enough. By placing an **extra set of angular brackets** at the start of your comment, you can bypass the filter and inject HTML that triggers Javascript, such as an alert.

## 🛠 Steps to Solve
1. Open the blog post comment section.
2. Inject the payload : `<><img scr=1 onerror=alert(1)>`
   
## 📖 Key Takeaways
- The site uses `replace()` improperly - it only escapes the **first instance**.
- **Stored XSS** means the payload is saved and is executed later in someone else's browser.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/2ef16b2d-85b8-4db6-8afd-d0524227d77e)
![image](https://github.com/user-attachments/assets/1051bda5-7f94-4f4a-ae0c-926f086b7bb4)
![image](https://github.com/user-attachments/assets/1f25d2a8-39a2-45ff-9393-dd9a304679f8)
