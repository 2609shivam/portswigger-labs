# XXE - Exploiting XInclude to retrieve files

## 📌 Lab Details
- **Title**: Exploiting XInclude to retrieve files
- **Difficulty**: Practitioner
- **Category**: XML external entity injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/68bb9b0d753781beeb1bd78ec3be309bed3de2f8b62b6336db6a92593bc5daaa?referrer=%2fweb-security%2fxxe%2flab-xinclude-attack

## 🔍 Summary
This lab shows how to exploit a vulnerability using **XInclude**, an XML feature that allows external content to be included in an XML document. Since the XML is partly controlled by the server, you can't inject a full DTD (like in classic XXE). Instead, you use an XInclude directive to read sensitive files like `/etc/passwd`.

## 🛠 Steps to Solve
1. Intercept the "Check stock" **POST** request in Burp Suite.
2. Set the value of `productId` parameter to:
   ```sh
   <foo xmlns:xi="http://www.w3.org/2001/XInclude">
   <xi:include parse="text" href="file:///etc/passwd"/>
   </foo>
   ```
3. Send the request and solve the lab.

## 📖 Key Takeaways
- **XInclude** can be used when you can't inject a full DTD.
- Injecting a special XML tag (`<xi:include>`) includes external file content into the server's XML processing.
- Add `parse="text"` because `/etc/passwd` is not valid XML.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f0d0b994-2a75-444e-b72c-83894333f4a8" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/95767e61-16f1-4b98-bafd-e950ba957843" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2a68355c-f7d0-4f42-a04d-6f8ed0e3562b" />
