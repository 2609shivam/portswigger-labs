#  File Path Traversal via Null Byte Extension Bypass

## 📌 Lab Details
- **Title**: File Path Traversal via Null Byte Extension Bypass
- **Difficulty**: Practitioner
- **Category**: Path Traversal
- **Lab URL**: https://portswigger.net/academy/labs/launch/563206d375aae19b45b636fbd08faf36892e8229b2f84d5908110bbdb7798561?referrer=%2fweb-security%2ffile-path-traversal%2flab-validate-file-extension-null-byte-bypass

## 🔍 Summary
The application is vulnerable to **directory traversal** in the product image functionality.
The server validates that the supplied filename ends with `.png`, but this validation can be bypassed using a **URL-encoded null byte (`%00`)**.

## 🛠 Steps to Solve
1. Use Burp Suite to intercept and modify a request that fetches a product image.\
2. Modify the filename parameter, giving it the value:
   ```sh
   filename=../../../etc/passwd%00.png
   ```
3. Observe that the response contains the contents of the `/etc/passwd` file.

## 📖 Key Takeaways
- Null bytes (`%00`) can bypass extension validation in vulnerable systems.
- Path traversal vulnerabilities often become exploitable when validation and filesystem handling behave differently.
- Never rely only on filename suffix checks for security.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/524c0dcf-5308-46fa-93b7-2af5c34bd1f4" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/40313992-ad47-4856-9786-d33a0226b849" />
