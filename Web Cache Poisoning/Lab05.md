#  Web Cache Poisoning to Exploit a DOM Vulnerability via a Cache with Strict Cacheability Criteria

## 📌 Lab Details
- **Title**: Web Cache Poisoning to Exploit a DOM Vulnerability via a Cache with Strict Cacheability Criteria
- **Difficulty**: Expert
- **Category**: Web Cache Poisoning
- **Lab URL**: https://portswigger.net/academy/labs/launch/c2431a0bc81d81ad810cc1a25bb19fba18e6fcc60b59e3c58fac444292f921d4?referrer=%2fweb-security%2fweb-cache-poisoning%2fexploiting-design-flaws%2flab-web-cache-poisoning-to-exploit-a-dom-vulnerability-via-a-cache-with-strict-cacheability-criteria

## 🔍 Summary
This lab combines **Web Cache Poisoning** and **DOM-Based XSS**. The application uses the `X-Forwarded-Host` header when constructing a URL for a JSON resource used by the geolocation feature. <br>
The value of this header is reflected into the response and eventually passed to a JavaScript function that fetches a JSON file. The JSON response is then inserted into the DOM using `innerHTML`, creating a DOM XSS sink. <br>
To solve the lab, we poison the cache so that all visitors receive a response referencing our malicious JSON file hosted on the exploit server. When the victim loads the poisoned page, the browser fetches the attacker-controlled JSON and executes the injected JavaScript. 

## 🛠 Steps to Solve
1. Find the `GET` request for the home page and send it to Burp Repeater.
2. Use Param Miner to identify that the `X-Forwarded-Host` header is supported.
3. Add a cache buster to the request, as well as the `X-Forwarded-Host` header with an arbitrary hostname, such as `example.com`. Notice that this header overwrites the data.host variable, which is passed into the `initGeoLocate()` function.
4. Study the `initGeoLocate()` function in `/resources/js/geolocate.js` and notice that it is vulnerable to DOM-XSS due to the way it handles the incoming JSON data.
5. Go to the exploit server and change the file name to match the path used by the vulnerable response: `/resources/json/geolocate.json`.
6. In the head, add the header `Access-Control-Allow-Origin: *` to enable CORS.
7. In the body, add a malicious JSON object that matches the one used by the vulnerable website. However, replace the value with a suitable XSS payload, for example:
   ```sh
   {
   "country": "<img src =x onerror=alert(document.cookie) />"
   }
8. Store the exploit.
9. In Burp Repeater, add the following header, remembering to enter your own exploit server ID: `X-Forwarded-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net`.
10. Send the request until you see your exploit server URL reflected in the response and `X-Cache: hit` in the headers.
11. To simulate the victim, load the URL in the browser and make sure that the `alert()` fires.
12. Replay the request to keep the cache poisoned until the victim visits the site and the lab is solved

## 📖 Key Takeaways
- Unkeyed Headers are dangerous.
- Cache Poisoning can deliver DOM XSS.
- External resources can become attack vectors.
- `innerHTML` creates DOM XSS risks.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c2e489c8-5f5d-4327-970e-cc7ee06180c6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0b671111-879b-4f81-b764-37642317a3ae" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/59e65225-52d8-4069-8331-3c77bf1a6aa3" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fa68d463-4ef2-4547-a471-83fdb6eebfb6" />
