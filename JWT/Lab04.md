#  JWT authentication bypass via jku header injection

## 📌 Lab Details
- **Title**: JWT authentication bypass via jku header injection
- **Difficulty**: Practitioner
- **Category**: Json Web Token
- **Lab URL**: https://portswigger.net/academy/labs/launch/01b6dad83328cfccc93032501b8d61bfc6683c5f5708bb2461495627003d1381?referrer=%2fweb-security%2fjwt%2flab-jwt-authentication-bypass-via-jku-header-injection

## 🔍 Summary
This lab is vulnerable to the JWT authentication bypass through the `jku` header parameter. The application allows the JWT to specify a remote JWK set URL and fails to verify whether the URL belongs to a trusted source. <br>
By hosting a malicious JWK set on the exploit server and signing a forget JWT with the matching private key, it is possible to impersonate the `admininistrator` user and gain access to the admin panel.

## 🛠 Steps to Solve
1. Login using the given credentials.
2. Capture the `GET /my-account` request. Try accessing the admin panel at `GET /admin`.
3. Generate an **RSA** key pair using the `JWT Editor`.
4. Upload a malicious JWK key set on the **exploit** server:
   ```sh
   {
       "keys": [
         {
           "kty": 
           "e":
           "kid":
           "n":
         }
    ]
   }
   ```
5. Open the JWT Editor tab for the `/admin` request. Modify the JWT header by including a `jku` parameter with the URL of the `exploit` server.
6. Sign the JWT and make sure to enable: **Don't Modify header**.
7. Send the modified request: `GET /admin`.
8. Delete the user carlos: `GET /admin?delete?username=carlos`.

## 📖 Key Takeaways
- Never trust user-controlled JWT headers.
- The 'jku' parameter can lead to authentication bypass if not validated properly.
- Applications should only fetch JWK Sets from trusted allowlisted domains.
- Signature verification becomes useless if attackers control the verification key.
- Misconfigured JWT implementation can lead to full priviledge escalation.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d2ab81ae-94ba-4253-b22b-c9a01b6f6ce6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0fb9ca87-f83f-410d-9a54-004a92a799d2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/568f39c0-9e77-4b35-9d0e-ac30e8c410e6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/20f80263-887e-44bf-a5b9-c276c807c80a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e60672dc-262d-4bee-9a82-dda55c585bf6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/631271ec-19d9-49f2-8a3e-34dd53f31968" />
