#  Web cache poisoning via an unkeyed query string

## 📌 Lab Details
- **Title**: Web cache poisoning via an unkeyed query string
- **Difficulty**: Practitioner
- **Category**: Web Cache Poisoning
- **Lab URL**: https://portswigger.net/academy/labs/launch/c3cbe5abaf85f50bfc76fb4c591a3811dcc7e393cbce37d987780b4c79ecf1c4?referrer=%2fweb-security%2fweb-cache-poisoning%2fexploiting-implementation-flaws%2flab-web-cache-poisoning-unkeyed-query

## 🔍 Summary
This lab demonstrates a web cache poisoning vulnerability caused by an **unkeyed query string**. The caching mechanism ignores query parameters when generating the cache key, but the application still processes and reflects them in the response. <br>
An attacker can inject malicious content through a query parameter, causing the poisoned response to be cached and subsequently served to other users who visit the same page without the malicious parameter. <br>
The goal is to poison the home page with an XSS payload that executes `alert(1)` in the victim's browser.

## 🛠 Steps to Solve
1. Find the `GET` request for the home page. Notice that this page is a potential cache oracle. Send the request to Burp Repeater.
2. Add arbitrary query parameters to the request. Observe that you can still get a cache hit even if you change the query parameters. This indicates that they are not included in the cache key.
3. Notice that you can use the `Origin` header as a cache buster. Add it to your request.
4. When you get a cache miss, notice that your injected parameters are reflected in the response. If the response to your request is cached, you can remove the query parameters and they will still be reflected in the cached response.
5. Add an arbitrary parameter that breaks out of the reflected string and injects an XSS payload:
   ```sh
   GET /?evil='/><script>alert(1)</script>
   ```
6. Keep replaying the request until you see your payload reflected in the response and `X-Cache: hit` in the headers.
7. To simulate the victim, remove the query string from your request and send it again (while using the same cache buster). Check that you still receive the cached response containing your payload.
8. Remove the cache-buster Origin header and add your payload back to the query string. Replay the request until you have poisoned the cache for normal users. Confirm this attack has been successful by loading the home page in the browser and observing the popup.
9. The lab will be solved when the victim user visits the poisoned home page. You may need to re-poison the cache if the lab is not solved after 35 seconds.

## 📖 Key Takeaways
- Cache keys must include all user-controlled inputs that influence the response.
- Ignoring query parameters while still processing them creates cache poisoning opportunities.
- Reflected input combined with caching can turn a self-XSS into a stored XSS affecting all users.
- Cache busters help attackers create and test isolated cache entries before targeting the public cache.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/eea022c4-25b0-4d50-8b03-dc24f1e93654" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b88da3ca-8e26-44ed-bf65-c6525dad5df0" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fd98f995-ab2e-49d5-99c1-e7604e463340" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c5101acc-7f1e-49f4-97d0-d5e780c20c4d" />
