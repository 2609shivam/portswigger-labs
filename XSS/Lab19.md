# XSS - Reflected XSS into a JavaScript string

## 📌 Lab Details
- **Title**: Reflected XSS into a JavaScript string with angle brackets and double quotes HTML-encoded and single quotes escaped
- **Difficulty**: Practitioner
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/1bf4b9250fa4c31d4444f2adf45736c70dd7e5515f1e35a50f3e5d1f74969d63?referrer=%2fweb-security%2fcross-site-scripting%2fcontexts%2flab-javascript-string-angle-brackets-double-quotes-encoded-single-quotes-escaped

## 🔍 Summary
This lab has a **reflected XSS** vulnerability inside a JavaScript string. Your input is reflected into a `<script>` block, and characters like angle brackets (`<>`) and double quotes (`""`) are HTML encoded and single quotes (`''`) are escaped. 

## 🛠 Steps to Solve
1. Submit a random alphanumeric string into the search box.
2. Use Burp Suite to observe that the input appears inside a `JavaScript` string.
3. Try entering `test'payload` and see how it's escaped.
4. Try entering `test\payload` and observed that backslash doesn't get escaped.
5. Inject the payload: `\'alert(1)//`.

## 📖 Key Takeaways
- **Angle brackets** (`<>`) and **double quotes** (`""`) are HTML encoded - Cannot use `<script>` or `""` directly.
- Single quotes (`'`) are escaped as `\'` - Can't break the string with single quote alone.
- **Backslashes (`\`) are NOT escaped** - This allows you to inject a literal `\'` and break the string.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/af14066f-f536-4d35-acb3-7d77682d5f79)
![image](https://github.com/user-attachments/assets/aa07455f-f81b-436f-842b-1a366d0a56c8)
![image](https://github.com/user-attachments/assets/9a60bc80-f7da-4624-bff4-221cb45f48e1)
![image](https://github.com/user-attachments/assets/4d98c8c4-a784-4b5e-9793-3c4d6674854d)
![image](https://github.com/user-attachments/assets/7fe8016e-f3ce-4109-9f14-745af49947bd)
![image](https://github.com/user-attachments/assets/33e24ab0-6ec6-4e5a-b565-795a8bf955b1)
![image](https://github.com/user-attachments/assets/9a6155d7-0cb3-4545-b082-131ce5be4c1b)
