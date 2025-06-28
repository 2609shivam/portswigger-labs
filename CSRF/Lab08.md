# CSRF - SameSite Strict bypass via client-side redirect

## 📌 Lab Details
- **Title**: SameSite Strict bypass via client-side redirect
- **Difficulty**: Practitioner
- **Category**: Cross-site Request Forgery
- **Lab URL**: https://portswigger.net/academy/labs/launch/0706592c0e05520a0d622e33957f791edd8348e91ea46cc2899bab8fd501e37b?referrer=%2fweb-security%2fcsrf%2fbypassing-samesite-restrictions%2flab-samesite-strict-bypass-via-client-side-redirect

## 🔍 Summary
This lab demonstrates bypassing **SameSite=Strict CSRF** protection using a **client-side redirect gadget** that turns a cross-site request into a same-origin navigation, allowing cookies to be sent. By injecting a **path traversal in a redirect parameter**, we force the victim’s browser to request the **email change endpoint via GET with their cookies**, successfully performing CSRF. This shows how client-side redirects can break SameSite Strict assumptions if endpoints lack CSRF tokens.

## 🛠 Steps to Solve
1. Log in to the given account.
2. Study the **POST change-email** request to observe that **no CSRF** tokens are being used.
3. Study the **POST login** request to observe that `SameSite=Strict` restriction is imposed.
4. Post a comment on a blog post to observe that you are being sent to a confirmation page, after a few seconds, you're taken back to the blog post.
5. Study the JavaScript and notice that this uses the `postId` query parameter to dynamically construct the path for the client-side redirect.
6. Try injecting a path traversal sequence so that the dynamically constructed redirect URL will point to your account page: `/post/comment/confirmation?postId=1/../../my-account`.
7. Craft an exploit for the exploit server:
   ```sh
   <script>
    document.location = "https://YOUR-LAB-ID.web-security-academy.net/post/comment/confirmation?postId=1/../../my-                account/change-email?email=pwned%40web-security-academy.net%26submit=1";
   </script>
   ```

## 📖 Key Takeaways
- **SameSite=Strict** blocks cookies on cross-site requests but not on same-origin redirected requests.
- **Client-side redirects** can convert **cross-site** navigation into a **same-origin** navigation.
- If CSRF endpoint accepts **GET + no CSRF tokens**, a redirect gadget can **bypass SameSite Strict**.
- **Path traversal** in redirect parameters (`../`) can reach sensitive endpoints.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/e877fdd2-a98a-481a-8095-fc298221e890)
![image](https://github.com/user-attachments/assets/5300ff70-af71-45c9-b266-75795129b11d)
![image](https://github.com/user-attachments/assets/20e64abf-60d8-476d-abf6-b43bfe0887d4)
![image](https://github.com/user-attachments/assets/a895fbb7-a3dd-4c25-8f2e-8fe6cf0a1871)
![image](https://github.com/user-attachments/assets/aaa67068-0383-49f8-9f21-87e436a17a5c)
![image](https://github.com/user-attachments/assets/c98a9a57-a1dd-405e-ae9c-4831f231693c)
![image](https://github.com/user-attachments/assets/844181f4-533d-4c00-8190-10fdf6b9a2dc)
![image](https://github.com/user-attachments/assets/a3b75d04-1abe-4c46-8ff3-89159e4c4558)
