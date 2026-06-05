#  Web cache poisoning via a fat GET request

## 📌 Lab Details
- **Title**: Web cache poisoning via a fat GET request
- **Difficulty**: Practitioner
- **Category**: Web Cache Poisoning
- **Lab URL**: https://portswigger.net/academy/labs/launch/445170885a5022d81beaf56ed8c332d3ce302fe5d37e99e61a0674d3ac3e6260?referrer=%2fweb-security%2fweb-cache-poisoning%2fexploiting-implementation-flaws%2flab-web-cache-poisoning-fat-get

## 🔍 Summary
This lab demonstrates a web cache poisoning vulnerability caused by a **fat GET request**. The application accepts a request body in a GET request and processes parameter from that body. However, the cache key is generated only from the URL and ignores the request body. <br>
As a result, an attacker can supply a malicious parameter in the GET body that changes the server response while the cache stores it under a key derived from the benign URL. Subsequent visitors receive the poisoned response from the cache. 

## 🛠 Steps to Solve
1. Observe that every page imports the script `/js/geolocate.js`, executing the callback function `setCountryCookie()`. Send the request to Burp Repeater.
2. Notice that you can control the name of the function that is called in the response by passing in a duplicate `callback` parameter via the request body. Also add the `Pragma: x-get-cache-key` header to observe that the cache-key is derived from the original `callback` parameter in the request line.
3. Send the request again, but this time pass in `alert(1)` as the callback function. Check that you can successfully poison the cache.
4. Remove any cache busters and re-poison the cache. The lab will solve when the victim user visits any page containing this resource import URL.

## 📖 Key Takeaways
- Some applications accept request bodies in GET requests (fat GET requests).
- Back-end parameter parsing may prioritize body parameters over URL parameters.
- If user-controlled input affects the response but is excluded from the cache key, cache poisoning becomes possible.
- JavaScript resources are high-value cache poisoning targets because poisoned content executes automatically in visitors' browsers.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/53523bbd-9cf4-4b67-865f-a4ac871851d3" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c3229650-a462-4ada-9d48-713603ab1ebf" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8f7347d6-5b13-4a29-a011-a09c545cac03" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2b9cde65-cc42-4d82-8d51-fd4a2ae973a1" />
