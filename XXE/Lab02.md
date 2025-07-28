# XXE - to perform SSRF attacks

## 📌 Lab Details
- **Title**: Exploiting XXE to perform SSRF attacks
- **Difficulty**: Apprentice
- **Category**: XML external entity injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/b856c63f6e47c5473568908b886232e76c6f1d02e9ebae4fd2ffe9d26adb1a56?referrer=%2fweb-security%2fxxe%2flab-exploiting-xxe-to-perform-ssrf

## 🔍 Summary
This lab demonstrates how an **XXE vulnerability** can be exploited to perform a **Server-Side Request Forgery (SSRF)**. The goal is to trick the vulnerable XML parser into making HTTP requests on behalf of the server to the **EC2 metadata endpoint** (`http://169.254.169.254/`) and extract the **AWS IAM Secret Access Key**.

## 🛠 Steps to Solve
1. Intercept the resulting **POST** request of "Check stock" in **Burp Suite**.
2. Insert the following **external entity** in between XML declaration:
   ```sh
   <!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin"> ]>
   ```
3. Replace the `productId` with a reference to the external entity `&xxe`.

## 📖 Key Takeaways
- You can abuse an **XXE vulnerability** to force the server to make **HTTP** requests internally (SSRF), not just access local files.
- On AWS EC2 instances, the endpoint `http://169.254.169.254/` exposes instance metadata.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ed38af41-a3ca-400d-a1c5-efe316c89db3" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bcf92dd8-78fe-464d-b2f4-2efc21156666" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1e9d4b7b-2086-4252-9434-5463a8afdcbb" />
