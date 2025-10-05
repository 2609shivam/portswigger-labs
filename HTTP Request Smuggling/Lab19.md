#  Web cache poisoning via HTTP/2 request tunnelling

## 📌 Lab Details
- **Title**: Web cache poisoning via HTTP/2 request tunnelling
- **Difficulty**: Expert
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/97e46bf5f67da1e25ef00c1a6414642112874ba9122dc3608c709510defce4f1?referrer=%2fweb-security%2frequest-smuggling%2fadvanced%2frequest-tunnelling%2flab-request-smuggling-h2-web-cache-poisoning-via-request-tunnelling

## 🔍 Summary
HTTP/2 request tunnelling lets you inject a second HTTP/1.1 request via the :path pseudo-header when the front end downgrades requests — that nested response can be cached by the front end and used to deliver reflected XSS to other users.

## 🛠 Steps to Solve
1. Send a request for `GET /` to Burp Repeater. Expand the Inspector's **Request Attributes** section and make sure the protocol is set to HTTP/2.
2. Using the Inspector, try smuggling an arbitrary header in the `:path` pseudo-header:<br>
   **Name**
   ```sh
   :path
   ```
   **Value**
   ```sh
   /?cachebuster=1 HTTP/1.1\r\n
   Foo: bar
   ```
3. Change the request method to `HEAD` and use the `:path` pseudo-header to tunnel a request for another arbitrary endpoint as follows:<br>
   **Name**
   ```sh
   :path
   ```
   **Value**
   ```sh
   / HTTP/1.1\r\n
   \r\n
   GET / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net\r\n
   \r\n
   ```
   Note that we've ensured that the main request is valid by including a `Host` header before the split. We've also left an arbitrary trailing header to capture the `HTTP/1.1` suffix that will be appended to the request line by the front-end during rewriting.
4. Now you need to find a gadget that reflects an HTML-based XSS payload without encoding or escaping it. Send a response for `GET /resources` and observe that this triggers a redirect to `/resources/`.
5. Try tunnelling this request via the `:path` pseudo-header, including an XSS payload in the query string as follows:
   **Name**
   ```sh
   path
   ```
   **Value**
   ```sh
   / HTTP/1.1\r\n
   \r\n
   GET /resources?<script>alert(1)</script> HTTP/1.1\r\n
   ```
   Observe that the request times out. This is because the `Content-Length` header in the main response is longer than the nested response to your tunnelled request.
6. From the proxy history, check the `Content-Length` in the response to a normal `GET /` request and make a note of its value. Go back to your malicious request in Burp Repeater and add enough arbitrary characters after the closing `</script>` tag to pad your reflected payload so that the length of the tunnelled response will exceed the `Content-Length` you just noted.
7. While the cache is still poisoned, visit the home page using the same cachebuster query parameter and confirm that the `alert()` fires.

## 📖 Key Takeaways
- **Attack vector**: HTTP/2 request tunnelling via the `:path` pseudo-header — you can inject a second HTTP/1.1 request after a valid downgraded request line.
- **Cache poisoning**: the front end can cache the backend’s nested response; poisoning that cache causes stored HTML (with your XSS) to be served to subsequent visitors.
- **Important trick**: use `HEAD` for the outer request and a `GET` in the tunneled payload, include a Host header before the split, and add arbitrary trailing headers so the backend accepts the nested request.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/16be8dbf-f3b7-47ac-9b9f-1a4a602eb743" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ceebebb1-6c89-4b38-9ceb-6fa582b16b50" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1f1df487-395b-4c38-ba3a-94906db292e5" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/dc3778c3-39fc-4808-99e7-ab5bae3a6e69" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0e11c0df-1a28-493f-9c04-722f9aff83fe" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/83a22b63-184d-4d4b-a541-2e5477d71095" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a86e7d3f-2ea7-443c-a020-76b01afda6e3" />
