# DOM - XSS using web messages and a JavaScript URL 

## 📌 Lab Details
- **Title**: DOM XSS using web messages and a JavaScript URL
- **Difficulty**: Practitioner
- **Category**: DOM-based vulnerability
- **Lab URL**: https://portswigger.net/academy/labs/launch/887f00fbadc4a46fc79d4827ebd012b3ba4d52ba2fef5b09a0a8b9a30a42a6de?referrer=%2fweb-security%2fdom-based%2fcontrolling-the-web-message-source%2flab-dom-xss-using-web-messages-and-a-javascript-url

## 🔍 Summary
This lab demonstrates **DOM-based open redirect via insecure** `postMessage` **handling**, using a `javascript:print()//http:` payload to bypass a flawed `indexOf` check and execute `print()` on the victim’s browser.

## 🛠 Steps to Solve
1. Notice that the home page contains an `addEventListener()` call that listens for a web message. The JavaScript contains a flawed `indexOf()` check that looks for the strings `"http:"` or `"https:"` anywhere within the web message. It also contains the sink `location.href`.
2. Inject the paylaod on the **Exploit** server:
   ```sh
   <iframe src="https://YOUR-LAB-ID.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:print()//http:','*')">
   ```  
3. Store the exploit and deliver it to the victim.
   
## 📖 Key Takeaways
- Shows bypassing weak URL validation** (`indexOf("http:")`) by appending `//http:` after a `javascript:` payload.
- Demonstrates DOM-based redirection XSS via `location.href` sink and `postMessage`.
- Highlights the need for strict URL validation (e.g., regex, allowlists) and avoiding `location.href` redirection with untrusted input.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/231e7785-0e29-4f82-946c-054f82231a19)
![image](https://github.com/user-attachments/assets/636c01fa-118a-40a7-9692-28951bbd012f)
![image](https://github.com/user-attachments/assets/481cdc47-45ae-4a47-8e4b-2a796c2f7312)
![image](https://github.com/user-attachments/assets/46f114cc-ef35-4c47-99b0-13000c770e7e)
![image](https://github.com/user-attachments/assets/c7b657ad-f0ee-4ef1-bdff-62eb117d31da)
