# Cache Key Injection

## 📌 Lab Details
- **Title**: Cache Key Injection
- **Difficulty**: Expert
- **Category**: Web Cache Poisoning
- **Lab URL**: https://portswigger.net/academy/labs/launch/998fb8feea47abfd2e2503c5ae92409006f19e4f2b2c6832bb01450185cefbb5?referrer=%2fweb-security%2fweb-cache-poisoning%2fexploiting-implementation-flaws%2flab-web-cache-poisoning-cache-key-injection

## 🔍 Summary
This lab demonstrates a complex exploit chain that combines multiple vulnerabilities:
1. Cache Key Injection
2. Unkeyed parameter handling
3. Client-Side Parameter Pollution (CSPP)
4. Resonse Header Injection (CRLF Injection)
5. Web Cache Poisoning
By chaining these issues together, an attacker can poison a cached JavaScript resource and force victims to execute arbitrary JavaScript, resulting in stored XSS.

## 🛠 Steps to Solve
1. Observe that the redirect at `/login` excludes the parameter `utm_content` from the cache key using a flawed regex. This allows you append arbitrary unkeyed content to the `lang` parameter: `/login?lang=en?utm_content=anything`
2. Observe that the page at `/login/` has an import from `/js/localize.js`. This is vulnerable to client-side parameter pollution via the `lang` parameter because it doesn't URL-encode the value.
3. Observe that the login page references an endpoint at `/js/localize.js` that is vulnerable to response header injection via the `Origin` request header, provided the `cors` parameter is set to `1`.
4. Use the `Pragma: x-get-cache-key` header to identify that the server is vulnerable to cache key injection, meaning the header injection can be triggered via a crafted URL.
5. Combine these four behaviors by poisoning the cache with following two requests:
   ```sh
   GET /js/localize.js?lang=en?utm_content=z&cors=1&x=1 HTTP/2
   Origin: x%0d%0aContent-Length:%208%0d%0a%0d%0aalert(1)$$$$

   GET /login?lang=en?utm_content=x%26cors=1%26x=1$$origin=x%250d%250aContent-Length:%208%250d%250a%250d%250aalert(1)$$%23 HTTP/2
   ```
   Note that the injected origin header is lower case to comply with the HTTP/2 specification.
6. This will poison `/login?lang=en` such that it redirects to a login page with a poisoned localization import that executes `alert(1)`, solving the lab.

## 📖 Key Takeaways
- Cache key injection occurs when attacker-controlled input influences cache key generation.
- Unkeyed parameters are dangerous, especially when applications remove them from cache keys using flawed regex patterns.
- Client-Side Parameter Pollution (CSPP) can occur when JavaScript constructs URLs using user-controlled parameters without applying encodeURIComponent().
- Response Header Injection (CRLF Injection) can be used to manipulate HTTP responses and inject attacker-controlled content into cached resources.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/675ceb79-4c53-46c7-bcb0-5373a529114e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/34a19fdf-0933-42f7-9164-2559c4247946" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fae71047-45a4-4545-b11a-c2cbdbbce9aa" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f6349acb-5249-47d2-8384-5daf2a8547e7" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0e5ee662-4765-4c54-ac03-db4c5053d515" />
