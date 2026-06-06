# Internal cache poisoning

## 📌 Lab Details
- **Title**: Internal cache poisoning
- **Difficulty**: Expert
- **Category**: Web Cache Poisoning
- **Lab URL**: https://portswigger.net/academy/labs/launch/ef5f16230d33fe616215fe47487299d39980a2a94f3d006b7fc3abe6b08837c1?referrer=%2fweb-security%2fweb-cache-poisoning%2fexploiting-implementation-flaws%2flab-web-cache-poisoning-internal

## 🔍 Summary
This lab demonstrates Internal Cache Poisoning in a multi-layer caching architecture.
The application uses:
- An external cache that keys requests using the query string.
- An internal cache that stores page fragments separately.
The X-Forwarded-Host header influences the generated URLs for several JavaScript resources. While the external cache includes this header in its cache key, the internal cache does not. <br>
As a result, an attacker can poison an internally cached fragment so that future visitors load attacker-controlled JavaScript, resulting in stored XSS.

## 🛠 Steps to Solve
1. Notice that the home page is a suitable cache oracle and send the `GET /` request to Burp Repeater.
2. Observe that any changes to the query string are always reflected in the response. This indicates that the external cache includes this in the cache key. Use Param Miner to add a dynamic cache-buster query parameter. This will allow you to bypass the external cache.
3. Observe that the X-Forwarded-Host header is supported. Add this to your request, containing your exploit server URL:
   ```sh
   X-Forwarded-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net
   ```
4. Send the request. If you get lucky with your timing, you will notice that your exploit server URL is reflected three times in the response. However, most of the time, you will see that the URL for the canonical link element and the `analytics.js` import now both point to your exploit server, but the `geolocate.js` import URL remains the same.
5. Keep sending the request. Eventually, the URL for the `geolocate.js` resource will also be overwritten with your exploit server URL. This indicates that this fragment is being cached separately by the internal cache. Notice that you've been getting a cache hit for this fragment even with the cache-buster query parameter - the query string is unkeyed by the internal cache.
6. Remove the `X-Forwarded-Host` header and resend the request. Notice that the internally cached fragment still reflects your exploit server URL, but the other two URLs do not. This indicates that the header is unkeyed by the internal cache but keyed by the external one. Therefore, you can poison the internally cached fragment using this header.
7. Go to the exploit server and create a file at `/js/geolocate.js` containing the payload `alert(document.cookie)`. Store the exploit.
8. Back in Burp Repeater, disable the dynamic cache buster in the query string and re-add the `X-Forwarded-Host` header to point to your exploit server.
9. Send the request over and over until all three of the dynamic URLs in the response point to your exploit server. Keep replaying the request to keep the cache poisoned until the victim user visits the page and the lab is solved.

## 📖 Key Takeaways
- Multiple cache layers can have differenet cache keys.
- A cache miss in one cache can still be a cache hit in another
- Fragment caching can be more dangerous than page caching.
- Headers can be dangerous cache-poisoning inputs.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c838e220-0520-4746-9ca8-10954cce03a5" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/80bed323-3d0f-44c4-9740-11bbfb5c0da2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ed47220c-6c73-4963-ba34-88f3e9c812ce" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b31911f7-6e9a-4c7f-bbc2-92bd4f876ce4" />
