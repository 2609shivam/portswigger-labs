# XSS - Reflected XSS in a JavaScript URL with some characters blocked

## 📌 Lab Details
- **Title**: Reflected XSS in a JavaScript URL with some characters blocked
- **Difficulty**: Expert
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/c29f5f30a0c79a94f6a847a63911bfbb403f8d60977b3e3b09110d166711a345?referrer=%2fweb-security%2fcross-site-scripting%2fcontexts%2flab-javascript-url-some-characters-blocked

## 🔍 Summary
This lab contains a **reflected XSS** vulnerability in the javascript URL with some characters blocked. In order to execute the payload we have to use the **exception handling** to call the **alert** function.

## 🛠 Payload Used
https://0a9e00bd034244e8803f039700dc0001.web-security-academy.net/post?postId=5&%27},x=x=%3E{throw/**/onerror=alert,1337},toString=x,window%2b%27%27,{x:%27

## 📖 Key Takeaways
- Even when characters like space are blocked, you can use **comments** (`/**/`) to **seperate tokens**.
- Overriding `toString()` lets you execute code during **coercion** like `window + ' '`.
- `onerror=alert` is a clever way to pop alerts **without directly calling alert()**.
- Arrow functions (`x => {}`) lets you define **blocks of code** that are **evaluated later**.
- Reflected XSS in `javascript:` URLs is dangerours because **execution happens on click**, not load.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/76a5169a-f05a-4096-bbcd-a644620c9741)
![image](https://github.com/user-attachments/assets/cbe3dbf8-4109-4ebc-bd30-9999ed2b4980)
