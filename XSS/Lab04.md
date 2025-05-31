# XSS - DOM XSS in innerHTML sink using source location.search

## 📌 Lab Details
- **Title**: DOM XSS in innerHTML sink using source location.search
- **Difficulty**: Apprentice
- **Category**: Cross-site scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/643e053e185d7855c6c7ea780bb082fa545fdd40e72aabc79f1742eb2ad07914?referrer=%2fweb-security%2fcross-site-scripting%2fdom-based%2flab-innerhtml-sink

## 🔍 Summary
The vulnerable JavaScript code sets the `innerHTML` property of a `<div>` element using untrusted input from `location.search`. This allows an attacker to inject malicious HTML and JavaScript.

## 🛠 Steps to Solve
1. Inject payload into the search box `<img src=1 onerror=alert(1)>`.
2. Click the search button.
3. The page URL will now contain your payload: `https://<your-lab-url>/?search=<img src=1 onerror=alert(1)>`.
4. The script is executed.
   
## 📖 Key Takeaways
- DOM XSS happens entirely on the client side.
- Sinks like `innerHTML`, `document.write`, and `eval()` are dangerous when fed user input.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/114219d2-37f7-408a-b827-4184dbbb90a6)
![image](https://github.com/user-attachments/assets/9a19d7ad-d830-400d-af5f-94c185a7fda2)
![image](https://github.com/user-attachments/assets/703ec5c6-7f9b-4ec7-8121-0a9347c3bda0)
