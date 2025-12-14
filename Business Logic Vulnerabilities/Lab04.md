#  Encryption Oracle

## 📌 Lab Details
- **Title**: Authentication Bypass via encryption oracle
- **Difficulty**: Practitioner
- **Category**: Business Logic
- **Lab URL**: https://portswigger.net/academy/labs/launch/6595c840a41181221c4f4f7c250580d7d9d5f9fa1b3d7aa1c9761ecdd2f33df5?referrer=%2fweb-security%2flogic-flaws%2fexamples%2flab-logic-flaws-authentication-bypass-via-encryption-oracle

## 🔍 Summary
The app exposes an **encryption oracle** and a **decryption oracle**. By using those two behaviours you can craft a cipher text that decrypts to `administrator:<timestamp>`, replace your `stay-logged-in` cookie with it, log in as admin, and delete the user `carlos`.

## 🛠 Steps to Solve
1. Log in with the "Stay logged in" option enabled and post a comment. Study the corresponding requests and responses using Burp's manual testing tools. Observe that the `stay-logged-in` cookie is encrypted.
2. Notice that when you try and submit a comment using an invalid email address, the response sets an encrypted `notification` cookie before redirecting you to the blog post.
3. Notice that the error message reflects your input from the email parameter in cleartext:<br> `Invalid email address: your-invalid-email` <br> Deduce that this must be decrypted from the notification cookie. Send the `POST /post/comment` and the subsequent `GET /post?postId=x` request (containing the notification cookie) to Burp Repeater.
4. In Repeater, observe that you can use the `email` parameter of the `POST` request to encrypt arbitrary data and reflect the corresponding ciphertext in the `Set-Cookie` header. Likewise, you can use the `notification` cookie in the `GET` request to decrypt arbitrary ciphertext and reflect the output in the error message. For simplicity, double-click the tab for each request and rename the tabs **encrypt** and **decrypt** respectively.
5. In the decrypt request, copy your `stay-logged-in` cookie and paste it into the `notification` cookie. Send the request. Instead of the error message, the response now contains the decrypted `stay-logged-in` cookie, for example:<br> `wiener:1598530205184` <br> This reveals that the cookie should be in the format `username:timestamp`. Copy the timestamp to your clipboard.
6. Go to the encrypt request and change the email parameter to `administrator:your-timestamp`. Send the request and then copy the new `notification` cookie from the response.
7. Decrypt this new cookie and observe that the 23-character "`Invalid email address: `" prefix is automatically added to any value you pass in using the `email` parameter. Send the `notification` cookie to Burp Decoder.
8. In Decoder, URL-decode and Base64-decode the cookie.
9. Select the first 23 bytes and delete them.
10. Re-encode the data and copy the result into the `notification` cookie of the decrypt request. When you send the request, observe that an error message indicates that a block-based encryption algorithm is used and that the input length must be a multiple of 16. You need to pad the "`Invalid email address: `" prefix with enough bytes so that the number of bytes you will remove is a multiple of 16.
11. In Burp Repeater, go back to the encrypt request and add 9 characters to the start of the intended cookie value, for example:<br> `xxxxxxxxxadministrator:<timestamp>` <br> Encrypt this input and use the decrypt the request to test that it can be successfully decrypted.
12. Send the new ciphertext to Decoder, then URL and Base64-decode it. This time, delete 32 bytes from the start of the data. Re-encode the data and paste it into the `notification` parameter in the decrypt request. Check the response to confirm that your input was successfully decrypted and, crucially, no longer contains the "`Invalid email address: `" prefix. You should only see `administrator:your-timestamp`.
13. From the proxy history, send the `GET /` request to Burp Repeater. Delete the `session` cookie entirely, and replace the `stay-logged-in` cookie with the ciphertext of your self-made cookie. Send the request. Observe that you are now logged in as the administrator and have access to the admin panel.
14. Using Burp Repeater, browse to `/admin` and notice the option for deleting users. Browse to `/admin/delete?username=carlos` to solve the lab.

## 📖 Key Takeaways
- Having both an encrypt endpoint and a decrypt reflection lets you encrypt arbitrary plaintext and verify/decrypt ciphertexts — a powerful primitive for forgery.
- **Byte/block alignment is critical**: Because the prefix length (23 bytes) isn’t block-aligned, you must add padding bytes so the prefix+padding occupy full blocks; then delete those whole blocks from the ciphertext so the remaining plaintext starts with your controlled value.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/34e4b3a0-c07d-4d31-9d77-9a7c6767df0b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f4136b11-63e9-41c0-952d-3a491541f53e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/643fa16b-1b1b-48a7-abfe-422ef15a735b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a36c3631-d435-48d3-9137-8df3a7dfc806" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9b2b68d8-6549-4bd0-837d-07dd665fad6a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/93fdabc0-75f4-4ff4-b30f-cb44b4dd25ff" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ffe82598-9e00-4b51-8810-54d425cbde14" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4369300c-b62e-4c5b-ab55-c36cd11b2853" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bae5e10a-2f78-41e5-a258-315edd6ee79d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/66c15b48-2d99-43bf-9135-4bd040c754e1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ceb76d1a-7c9d-4c62-81b5-72ab328ac4c4" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/dcef6132-67e5-4b50-8e69-0668ff8f12ee" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f8784c01-eab4-4c0a-9b15-e18a7823723a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/dad2974d-90e7-4edc-bc96-d66658f221f4" />
