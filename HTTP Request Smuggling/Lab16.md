#  HTTP/2 request splitting via CRLF injection

## 📌 Lab Details
- **Title**: HTTP/2 request splitting via CRLF injection
- **Difficulty**: Practitioner
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/d8e66f883c0efdd6cd4ccbbf6a654382ed2d13d5277bab587aa37904d5ec4211?referrer=%2fweb-security%2frequest-smuggling%2fadvanced%2flab-request-smuggling-h2-request-splitting-via-crlf-injection

## 🔍 Summary
This lab demonstrated **HTTP/2 request splitting via CRLF injection**. By injecting newline characters (`\r\n`) into an HTTP/2 header, we tricked the front-end into downgrading the request into **multiple HTTP/1.1 requests**. This allowed us to perform **response queue poisoning**: capturing an admin’s login response (including their session cookie). Using the stolen cookie, we accessed the admin panel and deleted `carlos`.

## 🛠 Steps to Solve
1. Send a request for `GET /` to Burp Repeater. Expand the Inspector's Request Attributes section and make sure the protocol is set to HTTP/2.
2. Change the path of the request to a non-existent endpoint, such as `/x`. This means that your request will always get a 404 response. Once you have poisoned the response queue, this will make it easier to recognize any other users' responses that you have successfully captured.
3. Using the Inspector, append an arbitrary header to the end of the request.
   **Name**
   ```sh
   foo
   ```
   **Value**
   ```sh
   bar\r\n
   \r\n
   GET /x HTTP/1.1\r\n
   Host: YOUR-LAB-ID.web-security-academy.net
   ```
4. Send the request. When the front-end server appends `\r\n\r\n` to the end of the headers during downgrading, this effectively converts the smuggled prefix into a complete request, poisoning the response queue.
5. Wait for around 5 seconds, then send the request again to fetch an arbitrary response. Most of the time, you will receive your own 404 response. If the attack is not successful then you can try sending a **sniper** attack through Burp Intruder to obtain a `302 Found` response.
6. Copy the session cookie and use it to send the following request:
   ```sh
   GET /admin HTTP/2
   Host: YOUR-LAB-ID.web-security-academy.net
   Cookie: session=STOLEN-SESSION-COOKIE
   ```
7. In the response, find the URL for deleting `carlos` (`/admin/delete?username=carlos`), then update the path in your request accordingly. Send the request to delete `carlos` and solve the lab.

## 📖 Key Takeaways
- **CRLF injection in HTTP/2 headers** enables request splitting during downgrade to HTTP/1.
- Poisoning the response queue lets you intercept other users’ responses (like admin login).
- Connection resets after ~10 requests, so reset with normal requests if things break.
- **Practical impact**: privilege escalation, cache poisoning, and session hijacking.
  
## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e680dbb3-1db5-40ac-b34b-cf7e24189862" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/360139e3-df5b-4461-a3fa-9d0fd3102585" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/22e9ab79-0580-442e-aa2d-e4c4d96248e2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c3452978-d173-4d80-a21a-0dac6df312c2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a69c9c17-ee75-42f8-a22d-1c252139cce5" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/dcce44be-c5af-4da6-bfb1-a7fabeb3dc45" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5ecd3565-777e-4001-b540-93c1b15bd70d" />
