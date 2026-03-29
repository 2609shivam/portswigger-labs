#  JWT authentication bypass via flawed signature verification

## 📌 Lab Details
- **Title**: JWT authentication bypass via flawed signature verification
- **Difficulty**: Apprentice
- **Category**: Json Web Token
- **Lab URL**: https://portswigger.net/academy/labs/launch/3390f79a2611b22e4fa8d437bd36fb902ea986c0fff45577c1543e0c634f3543?referrer=%2fweb-security%2fjwt%2flab-jwt-authentication-bypass-via-flawed-signature-verification

## 🔍 Summary
This lab demonstrates a vulnerability in JWT signature verification where the server accepts unsigned tokens. By modifying the JWT header to use `"alg":"none"` and removing the signature, an attacker can forge tokens and escalte privileges.

## 🛠 Steps to Solve
1. **Login** using the given credentials.
2. **Capture Request**
   - Intercept the request to `/my-account` in Burp Suite.
   - Observe that the session cookie is a JWT.
3. **Analyze JWT**
   - Decode the token in Burp Inspector.
   - Identify the `sub` claim containing the username (`wiener`).
4. **Send to Repeater**
   - Forward the request to Burp Repeater.
   - Change endpoint to `/admin` -> Access denied (not admin).
5. **Modify payload**
   ```json
   "sub": "administrator"
   ```
6. **Modify Header**
   ```json
   "alg": "none"
   ```
7. **Remove Signature**
   - Delete the signature part of JWT.
   - Keep trailing dot.
8. Send request to `/admin`.
9. Delete user by finding the endpoint: `/admin/delete?username=carlos`
    

## 📖 Key Takeaways
- JWT security depends on proper signature verification.
- Accepting `"alg:none"` is a critical misconfiguration.
- Never trust client-side tokens without validation.
- Always enforce allowed algorithms on the server side.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cb56a0e1-e37b-4935-8df2-31b9f7f99e2b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1ac67382-ba97-41d3-bdc3-94b6f3418cc1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a0a8ce85-b691-422f-9fd7-68fe8a486fd1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/727b6392-d751-4e58-ae31-9dd2b4685f0e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/201bdf69-3ff6-4b7a-bed3-8d21dd7e5b99" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e20e6b0d-b6e8-48b5-a75e-850210599d8d" />
