# JWT authentication bypass via jwk header injection 

## 📌 Lab Details
- **Title**: JWT authentication bypass via jwk header injection
- **Difficulty**: Practitioner
- **Category**: Json Web Token
- **Lab URL**: https://portswigger.net/academy/labs/launch/a0101cde2e9ba5d478cf9734acc8c05e0dd944dd7ecbdec01a99e17a008f7b42?referrer=%2fweb-security%2fjwt%2flab-jwt-authentication-bypass-via-jwk-header-injection

## 🔍 Summary
This lab demonstrates a vulnerability caused by trusting the `jwk` header inside a JWT. The server accepts an embedded JSON Web Key (`jwk`) provided by the user and uses it to verify the token signature without checking whether the key comes from a trusted source. By generating our own RSA key pair, embedding the public key in the JWT header, and signing the token using the matching private key, we can forge a valid token for the administrator user and gain access to the admin panel.

## 🛠 Steps to Solve
1. Log in using the provided credentials.
2. Capture the `GET /my-account` request in Burp Suite and send it to Repeater.
3. Change the request path to `/admin`.
4. Send the request and observe that access is denied because we are not the administrator.
5. Open the **JWT Editor** extension in Burp Suite.
6. Generate a new RSA key pair:
   - Go to **JWT Editor Keys**
   - Click **New RSA Key**
   - Click **Generate**
   - Save the generated key
7. Return to the request in Repeater and modify the payload: `"sub": "administrator"`.
8. At the bottom of the JWT editor:
   - Click **Attack**
   - Select **Embedded JWK**
   - Choose the RSA key generated earlier.
9. Burp automatically:
   - Adds a `jwk` parameter to the JWT header.
   - Signs the token using the private key.
10. Send the modified request.
11. Access to `/admin` is now granted because the server trusts the embedded `jwk`.
12. Send the request to delete `carlos`.

## 📖 Key Takeaways
- JWTs should never trust arbitary keys supplied by the client.
- The `jwk` header can be dangerous if the server does not validate the source of the key.
- Proper JWT implementations should:
  - Use trusted server-side keys only.
  - Reject untrusted embedded keys.
  - Validate `kid`, `jku` and `jwk` header carefully.
    

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8f271f86-379b-4664-b3f6-b6b8118ccd70" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e54d9112-76ac-41c2-b7c9-61fd79a05702" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5920f3c0-e0da-479c-b292-3a3ed8e57eea" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b86f08ca-63df-480c-8c65-515c8ec7d403" />
