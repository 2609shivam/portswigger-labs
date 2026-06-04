#  Web cache poisoning via an unkeyed query parameter

## 📌 Lab Details
- **Title**: Web cache poisoning via an unkeyed query parameter
- **Difficulty**: Practitioner
- **Category**: Web Cache Poisoning
- **Lab URL**: https://portswigger.net/academy/labs/launch/f14385ab65ba5ac2e5d347c09f32b51bd9ac1e55946c15aec01dd0f3e902a863?referrer=%2fweb-security%2fweb-cache-poisoning%2fexploiting-implementation-flaws%2flab-web-cache-poisoning-unkeyed-param

## 🔍 Summary
This lab demonstrates a web cache poisoning vulnerability caused by an **unkeyed query parameter**. Although most query parameters are included in the cache key, the application excludes certain analytics parameters from cache key generation. <br>
The parameter `utm_content` is reflected in the response but is not included in the cache key. As a result, an attacker can inject malicious content into a cached response and serve it to other users. <br>
The objective is to poison the cache with an XSS payload that executes `alert(1)` when the victim visits the homepage.

## 🛠 Steps to Solve
1. Observe that the home page is a suitable cache oracle. Notice that you get a cache miss whenever you change the query string. This indicates that it is part of the cache key. Also notice that the query string is reflected in the response.
2. Add a cache-buster query parameter `Origin=x` and a header to display cache-key response `Pragma: x-get-cache-key`.
3. Use Param Miner's "Guess GET parameters" feature to identify that the parameter `utm_content` is supported by the application.
4. Confirm that this parameter is unkeyed by adding it to the query string and checking that the **cache-key** remains the same.
5. Send a request with a utm_content parameter that breaks out of the reflected string and injects an XSS payload:
   ```sh
   GET /utm_contetn='/><script>alert(1)</script>
   ```
6. Once your payload is cached, remove the utm_content parameter, open the URL on the browser to check that the `alert()` is triggered when you load the page.
7. Remove your cache buster, re-add the `utm_content` parameter with your payload, and replay the request until the cache is poisoned for normal users. The lab will be solved when the victim user visits the poisoned home page.  

## 📖 Key Takeaways
- Not every query parameter is necessarily included in cache key generation.
- Cache Poisoning Requires an Input Mismatch.
- Reflection Alone Is Not Enough.
- Param Miner Is Useful Beyond Hidden Parameters.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7cc8979b-6f19-4f40-adaf-5534fb6b1dab" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6ddee3f6-47e8-4eb6-b190-d69d992e63bf" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/78e24bf6-5b59-409f-95d1-457d5dea701a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/02113d7b-4a70-465b-b04d-fb7e37287d15" />
