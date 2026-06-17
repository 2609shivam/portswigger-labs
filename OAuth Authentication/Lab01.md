# Forced OAuth profile linking

## 📌 Lab Details
- **Title**: Forced OAuth profile linking
- **Difficulty**: Practitioner
- **Category**: OAuth Authentication
- **Lab URL**: https://portswigger.net/academy/labs/launch/6277e415ef43d61680e72e8facbeceacf6e3e4957f85c443470fa06ad977b666?referrer=%2fweb-security%2foauth%2flab-oauth-forced-oauth-profile-linking

## 🔍 Summary
This lab demonstrates how an OAuth account-linking feature can be vulnerable to **Cross-Site Request Forgery (CSRF)** when the OAuth flow does not implement a **state parameter**. <br>
The application allows users to link a social-media account to their existing blog account. During the OAuth authorization flow, the application fails to include a state parameter, meaning an attacker can force another authenticated user to complete an OAuth linking request. <br>
By obtaining a valid authorization code for their own social media account and tricking the victim into visiting a malicious page, an attacker can link their social media profile to the victim's account. Once linked, the attacker can authenticate as the victim through the OAuth login functionality.

## 🛠 Steps to Solve
1. While proxying traffic through Burp, click "My account". You are taken to a normal login page, but notice that there is an option to log in using your social media profile instead. For now, just log in to the blog website directly using the classic login form.
2. Notice that you have the option to attach your social media profile to your existing account.
3. Click "Attach a social profile". You are redirected to the social media website, where you should log in using your social media credentials to complete the OAuth flow. Afterwards, you will be redirected back to the blog website.
4. Log out and then click "My account" to go back to the login page. This time, choose the "Log in with social media" option. Observe that you are logged in instantly via your newly linked social media account.
5. In the proxy history, study the series of requests for attaching a social profile. In the `GET /auth?client_id[...]` request, observe that the `redirect_uri` for this functionality sends the authorization code to `/oauth-linking`. Importantly, notice that the request does not include a `state` parameter to protect against CSRF attacks.
6. Turn on proxy interception and select the "Attach a social profile" option again.
7. Go to Burp Proxy and forward any requests until you have intercepted the one for `GET /oauth-linking?code=[...]`. Right-click on this request to copy the URL.
8. Drop the request. This is important to ensure that the code is not used and, therefore, remains valid.
9. Turn off proxy interception and log out of the blog website.
10. Go to the exploit server and create an `iframe` in which the `src` attribute points to the URL you just copied. The result should look something like this:
    ```sh
    <iframe src="https://YOUR-LAB-ID.web-security-academy.net/oauth-linking?code=STOLEN-CODE"></iframe>
    ```
11. Deliver the exploit to the victim. When their browser loads the `iframe`, it will complete the OAuth flow using your social media profile, attaching it to the admin account on the blog website.
12. Go back to the blog website and select the "Log in with social media" option again. Observe that you are instantly logged in as the admin user. Go to the admin panel and delete `carlos` to solve the lab.

## 📖 Key Takeaways
- OAuth account linking must always implement CSRF protection.
- The OAuth `state` parameter is specifically designed to prevent authorization response forgery.
- Authorization codes should be bound to the initiating user session.
- OAuth vulnerabilities often arise from insecure client-side implementation rather than flaws in the OAuth protocol itself.

## 🖼️ Screenshot 
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/e81dcd0c-f13c-4d77-9261-420aca7f2c06" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/41557548-0854-4138-abf0-bf94595944b8" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/eee29b9b-d2e6-4030-a418-3cd8d47fb58a" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/cd2a79c9-de88-4cb8-a6dc-89f9953dee50" />
