# DOM - XSS using web messages and JSON.parse

## 📌 Lab Details
- **Title**: DOM XSS using web messages and JSON.parse
- **Difficulty**: Practitioner
- **Category**: DOM-based vulnerability
- **Lab URL**: https://portswigger.net/academy/labs/launch/912c5f0d3d0d2905771da05ad007fd099d6fcd364cf93d4a3b855d2d64bdcaac?referrer=%2fweb-security%2fdom-based%2fcontrolling-the-web-message-source%2flab-dom-xss-using-web-messages-and-json-parse

## 🔍 Summary
This lab shows exploiting **web messaging with insecure JSON parsing** by sending a crafted `postMessage` payload that triggers a `load-channel` action, setting an iframe’s `src` to `javascript:print()` and executing JavaScript on the victim’s browser.

## 🛠 Steps to Solve
1. Notice that the home page contains an event listener that listens for a web message. This event listener expects a string that is parsed using `JSON.parse()`. In the JavaScript, we can see that the event listener expects a `type` property and that the `load-channel` case of the `switch` statement changes the `iframe src` attribute.
2. Inject the payload on the **Exploit** server:
   ```sh
   <iframe src=https://YOUR-LAB-ID.web-security-academy.net/ onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:print()\"}","*")'>
   ```
3. Store the exploit and deliver it to the victim.

## 📖 Key Takeaways
- Shows the risk of **allowing** `javascript:` **URLs in dynamic iframe assignments**.
- Demonstrates **DOM-based XSS via insecure** `postMessage` JSON parsing and sink assignment (`iframe.src = msg.url`).
- Highlights the need for **strict type validation, protocol whitelisting, and origin checks** when handling `postMessage` and JSON parsing in JavaScript.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/6399f2e4-300e-4463-a9f6-9be3ed40195d)
![image](https://github.com/user-attachments/assets/10e94da7-a79c-42b4-9440-7c2c866ff0f2)
![image](https://github.com/user-attachments/assets/ac9759c9-5c85-4b56-8531-30f910a76923)
![image](https://github.com/user-attachments/assets/d4579269-eb93-4818-9f47-af0a62632fdf)
![image](https://github.com/user-attachments/assets/29110433-8219-405b-8211-3bf3a1adc34e)
![image](https://github.com/user-attachments/assets/21225a11-f59c-4068-a954-2cffdbc22d65)
