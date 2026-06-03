#  Web cache poisoning with an unkeyed cookie

## 📌 Lab Details
- **Title**: Web cache poisoning with an unkeyed cookie
- **Difficulty**: Practitioner
- **Category**: Web Cache Poisoning
- **Lab URL**: https://portswigger.net/academy/labs/launch/d131a4d5f8a6a7e2ae584e9bc217208c223cad4bac3aed016482305228d68fb2?referrer=%2fweb-security%2fweb-cache-poisoning%2fexploiting-design-flaws%2flab-web-cache-poisoning-with-an-unkeyed-cookie

## 🔍 Summary
This lab is vulnerable to **Web Cache Poisoning** because the application reflects the value of a user-controlled cookie into the response, but the cookie is **not included in the cache key**. <br>
As a result, an attacker can inject malicious content into a cached response. Once cached, all users who receive that response will be served the attacker-controlled content, leading to stored XSS via the cache. 

## 🛠 Steps to Solve
1. Reload the home page and observe that the value from the `fehost` cookie is reflected inside a double-quoted JavaScript object in the response.
2. Send this request to Burp Repeater and add a cache-buster query parameter.
3. Change the value of the cookie to an arbitrary string and resend the request. Confirm that this string is reflected in the response.
4. Place a suitable XSS payload in the `fehost` cookie, for example:
   ```sh
   "}</script><script>alert(1)</script>
   ```
5. Replay the request until you see the payload in the response and `X-Cache: hit` in the headers.
6. Load the URL in the browser and confirm the `alert()` fires.
7. Go back Burp Repeater, remove the cache buster, and replay the request to keep the cache poisoned until the victim visits the site and the lab is solved.

## 📖 Key Takeaways
- **Cookies** can become a cache poisoning vector when their values influence the response but are **not included in the cache key**.
- Always identify user-controlled inputs that are reflected in responses.
- Cache behavior is just as important as input reflection
  
## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cd975df6-8150-47c7-8f9b-b67ec7655819" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a706f382-c458-4094-9e2f-fb0f827b5c69" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/be656a24-7b47-4ffc-8ede-f6ab243be406" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f38fc939-999a-48d5-b00d-8fdda85117b4" />
