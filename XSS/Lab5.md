# XSS - DOM XSS in jQuery anchor href attribute sink using location.search source

## 📌 Lab Details
- **Title**: DOM XSS in jQuery anchor href attribute sink using location.search source
- **Difficulty**: Apprentice
- **Category**: Cross-site scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/fde8784b9c158cac28b9d1a29d6331d2a28cf38742e1c59454be9a1fe37cff54?referrer=%2fweb-security%2fcross-site-scripting%2fdom-based%2flab-jquery-href-attribute-sink

## 🔍 Summary
Exploited a DOM-based XSS by injecting a `javascript`: URI into a `returnPath` parameter, causing the "Back" link to execute `alert(document.cookie)` when clicked.

## 🛠 Steps to Solve
1. Identified the vulnerable parameter: `returnPath`.
2. On the Submit feedback page modified the query parameter `returnPath` to `/abcd1234`.
3. Inspect the element and observe that the string has been placed inside an `href` attribute.
4. Changed `returnPath` to: `javascript:alert(document.cookie)`.
5. Hit Enter and click "Back". Payload is executed.
   
## 📖 Key Takeaways
- DOM XSS occurs when JavaScript modifies the DOM using unsanitized input from `location`, `document`, `hash` etc.
- `javascript`: URIs can be used to trigger execution in `href` attributes if input isn't sanitized.
- Even seemingly harmless attributes like `href` can become dangerous when they’re dynamically built.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/31979a10-5fa9-4ef7-afa1-ef3eef3686ed)
![image](https://github.com/user-attachments/assets/e9db7ae8-1d12-4e35-b411-27a4281dc0e4)
![image](https://github.com/user-attachments/assets/bc8bd514-c3db-4f20-80f3-6bdc6c96e9a5)
![image](https://github.com/user-attachments/assets/b0ce011d-700e-498f-955a-985332c62b14)
