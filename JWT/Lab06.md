#  JWT authentication bypass via algorithm confusion

## 📌 Lab Details
- **Title**: JWT authentication bypass via algorithm confusion
- **Difficulty**: Expert
- **Category**: Json Web Token
- **Lab URL**: https://portswigger.net/academy/labs/launch/9d708e568bf709fea5f6ee5ec1adee4a431698058d548d2aaac268a4ddb4e87a?referrer=%2fweb-security%2fjwt%2falgorithm-confusion%2flab-jwt-authentication-bypass-via-algorithm-confusion

## 🔍 Summary
This lab demonstrates a classic JWT algorithm confusion vulnerability where the server incorrectly trusts the `alg` header supplied by the client.<br>
The application normally uses RSA (`RS256`) to sign JWTs. However, during verification, the server dynamically switches between asymmetric and symmetric verification depending on the attacker-controlled `alg` value inside the JWT header. <br>
Because the server exposes its RSA public key through `/jwks.json`, an attacker can obtain the public key and abuse it as an HMAC secret key by changing the algorithm from `RS256` to `HS256`.

## 🛠 Steps to Solve
### Part 1 - Obtain the server's public key
1. Log in to your account and capture the request: `GET /my-account`.
2. Change the request to `/admin` and observe that admin panel is only accessible when logged in as the `administrator` user.
3. In the browser go to the standard endpoint `/jwk.json` and observe that the server exposes a JWK Set containing a single public key.
4. Copy the JWK object from inside the `keys` array.

### Part 2 - Generate a malicious signing key
1. Go to the **JWK Editor Keys** tab.
2. Click **New RSA** Key. In the dialog, paste the JWK that you obtained earlier.
3. Select the **PEM** radio button and copy the resulting PEM key.
4. Create a **New Symmetric Key** in JWK format.
5. Replace the generated value for the `k` parameter with a Base64-encoded PEM key that you just copied.

### Part 3 - Modify and sign the token
1. Switch to the extension-generated **JSON Web Token** tab.
2. Change the value of the `alg` parameter to `HS256`.
3. Change the value of the `sub` claim to `administrator`.
4. Sign the token using the generated symmetric key.
5. Send the request to access the admin panel.
6. Delete the user **carlos** to complete the lab.

## 📖 Key Takeaways
- Servers should never dynamically switch verification methods based on attacker-controlled input.
- Public keys must never become HMAC Secrets.
- JWKS Endpoints increase attack surface.

## 🖼️ Screenshot 
<img width="1919" height="291" alt="image" src="https://github.com/user-attachments/assets/c781c48b-9e36-4bdd-baf6-127b2e497fee" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9cb1075a-81fc-400f-83ff-25a0aecf9b23" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/119a3cf3-9929-46c4-9422-4524868e93a2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2c6e93d7-61ef-4565-b264-d8a3d8c5e170" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fd5b68af-646f-4d21-b9e9-93ab2820197f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/22100f42-e3ab-477d-9075-641998cbf89d" />
