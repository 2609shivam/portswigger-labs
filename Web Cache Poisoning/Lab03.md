# Web Cache Poisoning with Multiple Headers

## 📌 Lab Details
- **Title**: Web Cache Poisoning with Multiple Headers
- **Difficulty**: Practitioner
- **Category**: Web Cache Poisoning
- **Lab URL**: https://portswigger.net/academy/labs/launch/ef8228ac8678117cd722c596ac9f2f9e76bbf7d6d2f09a53ce7cf28d04ccff7a?referrer=%2fweb-security%2fweb-cache-poisoning%2fexploiting-design-flaws%2flab-web-cache-poisoning-with-multiple-headers

## 🔍 Summary
This lab demonstrates a web cache poisoning vulnerability that requires the attacker to manipulate multiple HTTP headers simultaneously. <br>
The application uses both the `X-Forwarded-Host` and `X-Forwarded-Scheme` headers when generating redirects. By combining these headers, it is possible to force the application to generate a redirect pointing to an attacker-controlled domain. <br>
Because the resulting response is cached, all subsequent visitors receive the poisoned response, causing them to load malicious JavaScript from the attacker's server.

## 🛠 Steps to Solve
1. Find the `GET` request for the JavaScript file `/resources/js/tracking.js` and send it to Burp Repeater.
2. Add a cache-buster query parameter and the `X-Forwarded-Host` header with an arbitrary hostname, such as `example.com`. Notice that this doesn't seem to have any effect on the response.
3. Remove the `X-Forwarded-Host` header and add the `X-Forwarded-Scheme` header instead. Notice that if you include any value other than `HTTPS`, you receive a 302 response. The `Location` header shows that you are being redirected to the same URL that you requested, but using `https://`.
4. Add the `X-Forwarded-Host: example.com` header back to the request, but keep `X-Forwarded-Scheme: nothttps` as well. Send this request and notice that the Location header of the 302 redirect now points to `https://example.com/`.
5. Go to the exploit server and change the file name to match the path used by the vulnerable response: `/resources/js/tracking.js`
6. In the body, enter the payload `alert(document.cookie)` and store the exploit.
7. Go back to the request in Burp Repeater and set the `X-Forwarded-Host` header as follows:
   ```sh
   X-Forwarded-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
   ```
8. Make sure the `X-Forwarded-Scheme` header is set to anything other than `HTTPS`.
9. Send the request until you see your exploit server URL reflected in the response and `X-Cache: hit` in the headers.
10. Go back to Burp Repeater, remove the cache buster, and resend the request until you poison the cache again.
11. To simulate the victim, reload the home page in the browser and make sure that the `alert()` fires.
12. Keep replaying the request to keep the cache poisoned until the victim visits the site and the lab is solved.

## 📖 Key Takeaways
- Multiple individually harmful headers can become dangerous when combined.
- Cache poisoning often relies on subtle interactions between application logic and caching behavior.
- Redirect responses can be valuable cache poisoning targets.
- Always investigate how caches handle redirects and forwarding headers.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fee3e574-e133-4395-9457-357f4c1b673b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/31baf3ad-6c5b-471a-aedd-c69b855844c2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/45806ea4-1bfc-48de-87a2-a2d77772a487" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0bcc12fd-8263-41d1-9426-858b6134d1c1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/18e45ede-1b99-4a73-ac15-a2372ce75643" />
