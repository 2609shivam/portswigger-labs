#  Parameter cloaking

## 📌 Lab Details
- **Title**: Parameter cloaking
- **Difficulty**: Practitioner
- **Category**: Web Cache Poisoning
- **Lab URL**: https://portswigger.net/academy/labs/launch/0e7fb21969d7178a1c1ae847cd2b80f7a277dee996bd1450604ba28d8d7a25da?referrer=%2fweb-security%2fweb-cache-poisoning%2fexploiting-implementation-flaws%2flab-web-cache-poisoning-param-cloaking

## 🔍 Summary
This lab demonstrates **parameter cloaking**, a web cache poisoning technique where a parameter excluded from the cache key can be abused to hide additional parameters from the cache while still being processed by the backend. <br>
The application excludes the analytics parameter `utm_content` from the cache key. By exploiting differences in parameter parsing between the cache and backend, an attacker can smuggle a second parameter inside `utm_content` using a semicolon (`;`). <br>
This allows an attacker to override a cache-keyed parameter without affecting the cache key, resulting in a poisoned response being served to victims.

## 🛠 Steps to Solve
1. Identify that the `utm_content` parameter is supported. Observe that it is also excluded from the cache key.
2. Notice that if you use a semicolon (`;`) to append another parameter to `utm_content`, the cache treats this as a single parameter.
3. Observe that every page imports the script `/js/geolocate.js`, executing the callback function `setCountryCookie()`. Send the request to Burp Repeater.
4. Notice that you can control the name of the function that is called on the returned data by editing the `callback` parameter. However, you can't poison the cache for other users in this way because the parameter is keyed.
5. Study the cache behavior. Observe that if you add duplicate `callback` parameters, only the final one is reflected in the response, but both are still keyed. However, if you append the second `callback` parameter to the `utm_content` parameter using a semicolon, it is excluded from the cache key and still overwrites the callback function in the response.
6. Send the request again, but this time pass in `alert(1)` as the callback function:
   ```sh
   GET /js/geolocate.js?callback=setCountryCookie&utm_content=foo;callback=alert(1)
   ```
7. Get the response cached, then load the home page in the browser. Check that the `alert()` is triggered.
8. Replay the request to keep the cache poisoned. The lab will solve when the victim user visits any page containing this resource import URL.

## 📖 Key Takeaways
- A parameter excluded from the cache key can be used to hide additional parameters that the backend still processes.
- JSONP-style callbacks are particularly dangerous.
- Unkeyed inputs are dangerous.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a417eac8-5c9e-443c-a489-39c3f0dfdd6f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fba3ce96-a6c7-49b4-86f4-9a7f18199589" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/21ce5427-3822-40cd-abf0-322c6d998790" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a61573db-b70f-491b-869c-bb06c89312a6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3e5d9dfe-6206-439b-aaaf-8c7ce5eef73f" />
