# XSS - DOM XSS in document.write sink using source location.search

## 📌 Lab Details
- **Title**: DOM XSS in document.write sink using source location.search
- **Difficulty**: Apprentice
- **Category**: Cross-site scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/6127df9244c9acedc241085d647719a0d9975c5af692444a9c913566fd2fd88b?referrer=%2fweb-security%2fcross-site-scripting%2fdom-based%2flab-document-write-sink

## 🔍 Summary
This lab demonstrated a DOM based XSS vulnerability and how it can be used to inject a `svg` tag into the backend.

## 🛠 Steps to Solve
1. Entered a random alphanumeric string into the search box.
2. Right click and inspect the element to observe that the random string has been placed inside an `img src` attribute.
3. Break out of the `img` attribute by serching for `"><svg onload=alert(1)>`.
   
## 📖 Key Takeaways
- DOM XSS happens entirely in the browser, using client-side JavaScript.
- Functions like document.write(), innerHTML, and eval() are common XSS sinks.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/dd32ce40-e5f0-44ca-bf26-82720e5f9e96)
![image](https://github.com/user-attachments/assets/ddc774e2-5e87-4d1b-80bc-57a3ebcad104)
