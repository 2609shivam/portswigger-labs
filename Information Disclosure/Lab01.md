#  Authentication bypass via information disclosure

## 📌 Lab Details
- **Title**: Authentication bypass via information disclosure
- **Difficulty**: Apprentice
- **Category**: Information Disclosure
- **Lab URL**: https://portswigger.net/academy/labs/launch/2c1ffa3c35d94beac72d213f5134c9b53f43a31758c8a61bc1a4d3bd8153c226?referrer=%2fweb-security%2finformation-disclosure%2fexploiting%2flab-infoleak-authentication-bypass

## 🔍 Summary
This lab demonstrates how information disclosure can reveal internal security mechanisms that can then be abused to bypass authentication.
The `/admin` endpoint is protected so that only administrators or requests originating from localhost can access it. However, a custom HTTP header used by the front-end is unintentionally disclosed through the HTTP TRACE method. By discovering and manipulating this header, an attacker can spoof a localhost request and gain unauthorized access to the admin panel.

## 🛠 Steps to Solve
1. In Burp Repeater, browse to GET /admin. The response discloses that the admin panel is only accessible if logged in as an administrator, or if requested from a local IP.
2. Send the request again, but this time use the `TRACE` method: `TRACE /admin`.
3. Study the response. Notice that the `X-Custom-IP-Authorization` header, containing your IP address, was automatically appended to your request. This is used to determine whether or not the request came from the `localhost` IP address.
4. Customize the **Match and replace** feature of Burp Proxy to add the custom header to all the requests. In the **Replace** field, enter the following:
   ```sh
   X-Custom-IP-Authorization: 127.0.0.1
   ```
5. Under Auto-modified request, notice that Burp has added the `X-Custom-IP-Authorization` header to the modified request.
6. Now browser to `/admin` to delete the user carlos and solve the lab.

## 📖 Key Takeaways
- Information disclosure often serves as the first step in a larger attack chain.
- The HTTP `TRACE` method can reveal sensitive request headers and internal implementation details.
- Trusting client-controlled headers for security decisions is dangerous.
- IP-based access controls can often be bypassed when the application relies on spoofable headers.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5b3d6915-5d80-4f6f-a009-d2d21b40303b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7afd90d2-791f-4b8f-9abe-2562bc1a1572" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/45b5d418-400a-44ec-98fd-b93ec1f84c68" />
