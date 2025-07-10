# Exploiting DOM clobbering to enable XSS

## 📌 Lab Details
- **Title**: Exploiting DOM clobbering to enable XSS
- **Difficulty**: Expert
- **Category**: DOM-based vulnerability
- **Lab URL**: https://portswigger.net/academy/labs/launch/992c042f1d4d37ec3fc36d4dd46f47d9769928fd769e125ae22c02fbf99177b4?referrer=%2fweb-security%2fdom-based%2fdom-clobbering%2flab-dom-xss-exploiting-dom-clobbering

## 🔍 Summary
This lab demonstrates **DOM clobbering via “safe” HTML injection**, using crafted `<a>` tags to overwrite a global defaultAvatar object. By injecting `cid:"onerror=alert(1)//`, the next page load triggers `alert(1)` due to the `onerror` event, achieving XSS in Chrome

## 🛠 Steps to Solve
1. Go to one of the blog posts and create a comment containing the following anchors:
   ```sh
   <a id=defaultAvatar><a id=defaultAvatar name=avatar href="cid:&quot;onerror=alert(1)//">
   ```
2. Return to the blog post and create a second comment containing any random text. The next time the page loads, the `alert()` is called.

## 📖 Key Takeaways
- DOM clobbering **can bypass XSS filters by overwriting variables with DOM collections using** `id` and `name` attributes.
- Using **two `<a>` tags with the same id creates a DOM collection**, clobbering the variable.
- Abuse of the `cid:` **protocol with unencoded double quotes allows XSS even with DOMPurify present**.
- Highlights **why relying on global variables and unsafe patterns** (`window.var || {}`) is dangerous in JavaScript.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/a643d85d-e98c-49a9-825f-18562f97b3b2)
![image](https://github.com/user-attachments/assets/ca9cf761-3a20-45dd-bd1f-9ba2bfb221c9)
![image](https://github.com/user-attachments/assets/4d393dfd-d099-42c2-88e7-1c6564bad9d2)
