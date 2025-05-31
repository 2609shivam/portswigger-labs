# XSS - Reflected XSS in canonical link tag

## 📌 Lab Details
- **Title**: Reflected XSS in canonical link tag
- **Difficulty**: Practitioner
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/69d9a58b4be85fe3da0e5e65bb0b6655278492c77fce16b6989c5b40481c66ed?referrer=%2fweb-security%2fcross-site-scripting%2fcontexts%2flab-canonical-link-tag

## 🔍 Summary
This lab contains a **reflected XSS** vulnerability inside a `<link="canonical">` tag. Although **angle brackets** are escaped, you can still inject **HTML attributes** such as `accesskey` and `onclick`.

## 🛠 Steps to Solve
1. Modify the lab URL: `https://0a360051038608d4809c3fad006c00e6.web-security-academy.net/?%27accesskey=%27x%27onclick=%27alert(1)`.
2. Press the following key combination: `ALT+SHIFT+X`.
3. An `alert(1)` is triggered which completes the lab.

## 📖 Key Takeaways
- XSS doesn't always need a `<script>` tag.
- You can break out of HTML attributes using **quotes**.
- HTML attributes like `accesskey` + `onclick` can be chained for keyboard-triggered attacks.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/d273a69e-147b-419a-befa-878276ae6b49)
![image](https://github.com/user-attachments/assets/7dcb2cf8-1f30-427f-af0d-d53380893575)
![image](https://github.com/user-attachments/assets/65b07a57-c264-4e9a-bc74-d6c2233b7a74)
