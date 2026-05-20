#  File Path Traversal via Superfluous URL-Decoding

## 📌 Lab Details
- **Title**: File Path Traversal via Superfluous URL-Decoding
- **Difficulty**: Practitioner
- **Category**: Path Traversal
- **Lab URL**: https://portswigger.net/academy/labs/launch/0ff4cb8691cfff7dc5d2c45682d859b9a5c3ba47cb98e42aa05a3391cd0cfc03?referrer=%2fweb-security%2ffile-path-traversal%2flab-superfluous-url-decode

## 🔍 Summary
This lab demonstrates a **path traversal vulnerability** caused by improper input sanitization combined with an additional URL-decoding step.
The application attempts to block traversal sequences like: `../`.
However, after filtering, the server performs another URL decode operation. By sending **double URL-encoded traversal sequences**, it becomes possible to bypass the filter and access arbitrary files on the server.

## 🛠 Steps to Solve
1. Use Burp Suite to intercept and modify a request that fetches a product image.
2. Modify the filename parameter, giving it the value:
   ```sh
   ..%252f..%252f..%252fetc/passwd
   ```
3. Observe that the response contains the contents of the `/etc/passwd` file.

## 📖 Key Takeaways
- Filtering user input before canonicalization is dangerous.
- Double URL encoding is a common technique for bypassing weak filters.
- Applications should:
  - fully decode input first
  - normailize paths
  - validate against an allowlist

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/791721b7-d08e-47d4-8f12-6f6cc37d5875" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bb3112d5-79e0-4cc6-8a94-2cf289f3cf23" />
