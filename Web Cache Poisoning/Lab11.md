#  URL Normalization

## 📌 Lab Details
- **Title**: URL Normalization 
- **Difficulty**: Practitioner
- **Category**: Web Cache Poisoning
- **Lab URL**: https://portswigger.net/academy/labs/launch/da7b121cfc61090ed5e7cecb6c6f31b595087c04ea02056591225cf659e72eec?referrer=%2fweb-security%2fweb-cache-poisoning%2fexploiting-implementation-flaws%2flab-web-cache-poisoning-normalization

## 🔍 Summary
This lab demonstrates how discrepancies between browser URL encoding and cache URL normalization can be abused to perform Web Cache Poisoning and trigger a reflected XSS vulnerability. <br>
The application reflects the requested URL path inside an error page. Normally, browsers URL-encode dangerous characters such as `<` and `>`, preventing direct exploitation. However, the cache normalizes (URL-decodes) the path before storing and serving cached responses. <br>
An attacker can poison the cache using a crafted request containing an XSS payload. When a victim later requests the same URL through their browser, the cache returns the decoded version, causing the JavaScript payload to execute.

## 🛠 Steps to Solve
1. In Burp Repeater, browse to any non-existent path, such as `GET /random`. Notice that the path you requested is reflected in the error message
2. Add a suitable reflected XSS payload to the request line:
   ```sh
   GET /random</p><script>alert(1)</script>
   ```
3. Notice that if you request this URL in the browser, the payload doesn't execute because it is URL-encoded.
4. In Burp Repeater, poison the cache with your payload and then immediately load the URL in the browser. This time, the `alert()` is executed because the browser's encoded payload was URL-decoded by the cache, causing a cache hit with the earlier request.
5. Re-poison the cache then immediately go to the lab and click "Deliver link to victim". Submit your malicious URL. The lab will be solved when the victim visits the link.

## 📖 Key Takeaways
- Browsers automatically URL-encode special characters in URLs.
- Some caches normalize URLs before generating cache keys.
- URL normalization can create inconsistencies between browsers, caches, and back-end applications.
- Reflected XSS vulnerabilities that appear unexploitable may become exploitable through cache poisoning.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4e96db18-91b7-4b74-a6f2-4071b4d6dd12" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7ea4892f-5aaa-437b-a0c8-259c6721153b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7775c078-e415-4f9e-a387-9de075b746ab" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/997f2001-b5d8-4973-ade0-15b5a477a5ee" />
