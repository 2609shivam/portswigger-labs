#  Password Reset Poisoning via Middleware

## 📌 Lab Details
- **Title**: Password Reset Poisoning via Middleware
- **Difficulty**: Practitioner
- **Category**: HTTP Host header attacks
- **Lab URL**: https://portswigger.net/academy/labs/launch/eb59e3d1483ff8122d7fde75e8e54fc95160ec5c56abdf09f3ef419b02939a4c?referrer=%2fweb-security%2fauthentication%2fother-mechanisms%2flab-password-reset-poisoning-via-middleware

## 🔍 Summary
This lab demonstrates a **Password Reset Poisioning** vulnerability caused by trusting user-controlled proxy headers when generating password reset links. <br>
The application uses the `X-Forwarded-Host` header to construct the password reset URL that is emailed to users. Since this header is not properly validated, an attacker can manipulate the generated link and cause the victim's password reset token to be sent to an attacker-controlled domain. 

## 🛠 Steps to Solve
1. With Burp running, investigate the password reset functionality. Observe that a link containing a unique reset token is sent via email.
2. Send the `POST /forgot-password` request to Burp Repeater. Notice that the `X-Forwarded-Host` header is supported and you can use it to point the dynamically generated reset link to an arbitrary domain.
3. Go back to the request in Burp Repeater and add the `X-Forwarded-Host` header with your exploit server URL.
4. Change the `username` parameter to `carlos` and send the request.
5. Go to the exploit server and open the access log. You should see a `GET /forgot-password` request, which contains the victim's token as a query parameter. Make a note of this token.
6. Send a valid password reset link with the `temp-forgot-password-token` of the victim.
7. Load this URL and set a new password for Carlos's account.
8. Log in to Carlos's account using the new password to solve the lab.

## 📖 Key Takeaways
- Password reset functionality is a high-value attack target.
- User-controlled proxy headers should never influence security-critical URLs.
- Stealing a password reset token is often equivalent to stealing the account.
- Always verify how applications construct absolute URLs in emails.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4154e4e9-9673-4274-b9ab-cf89b8ae151c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c91b4473-1038-4d6b-a369-3e9b388dbc6a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2176cfe1-77f9-457e-9d0b-6a423147e3b6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1c05c0be-1ad9-4eb3-9602-dd4c50dcf39b" />
