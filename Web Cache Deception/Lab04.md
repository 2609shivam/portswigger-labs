# Exploiting exact-match cache rules for web cache deception

## 📌 Lab Details
- **Title**: Exploiting exact-match cache rules for web cache deception
- **Difficulty**: Expert
- **Category**: Web Cache Deception
- **Lab URL**: https://portswigger.net/academy/labs/launch/988579f4180d533b4ca2e244a488d7d252f5b1aef28508ea8c5284936740d9ac?referrer=%2fweb-security%2fweb-cache-deception%2flab-wcd-exploiting-exact-match-cache-rules

## 🔍 Summary
This lab demonstrates a **Web Cache Deception** vulnerability caused by discrepancies between:
1. The **origin server's path parsing behavior**
2. The **cache's URL normailization**
3. An **exact-match cache rule** based on a specific filename (`robots.txt`)
The origin server treats `;` as a path delimiter and ignores everything after it when routing requests, allowing access to `/my-account`. <br>
The cache, however, normalizes encoded path traversal sequences and evaluates only the final filename. As a result, a request for:
```http
An exact-match cache rule based on a specific filename (robots.txt)
```
is processed by:
- Origin -> `/my-account`
- Cache -> `/robots.txt`
This causes sensitive account pages to be cached under a publicly cacheable filename. <br>
The attacker first tricks the administrator into visiting the malicious URL, then retrieves the cached response containing the administrator's CSRF token and uses it to perform a CSRF attack that changes the administrator's email address.

## 🛠 Steps to Solve
### Identify a target endpoint
1. In browser, log in using the given credentials.
2. Notice that the email change submission form in the `/my-account` response contains a CSRF token as a hidden parameter.

### Investigate path delimiter discrepancies
1. Send the `GET /my-account` request to Burp Repeater.
2. In Repeater, change the URL path to `/my-account/abc`, then send the request. Notice the `404 Not Found` response. This indicates that the origin server doesn't abstract the path to `/my-account`.
3. Change the path to `/my-accountabc`, then send the request. Notice that this returns a `404 Not Found` response with no evidence of caching.
4. In Intruder, craft an attack to identify whether the origin server uses any path delimiters. Use the payload: `/my-account§§abc`. Notice that `;` and `?` are both used as delimiters.
5. Go to the Repeater tab that contains the `/my-account/abc` request. Update the path to `/my-account?abc.js`, then send the request. Notice that the response doesn't contain evidence of caching.
6. Repeat this test using the `;` character instead of `?`. Notice that the response doesn't show evidence of caching.

### Investigate normalization discrepancies
1. Add an arbitrary directory followed by an encoded dot-segment to the start of the original path. For example, `/aaa/..%2fmy-account`.
2. Send the request. Notice that this receives a `404` response. This indicates that the origin server doesn't decode or resolve the dot-segment to normalize the path to `/my-account`.
3. Notice that static resources share the URL path directory prefix `/resources`. Notice that none of these show evidence of being cached. This indicates that there isn't a static directory cache rule.
4. Change the URL path of the `/my-account` request to `/robots.txt`.
5. Send the request. Notice that the response contains the `X-Cache: miss` header. Resend and notice that this updates to `X-Cache: hit`. This indicates that the cache has a rule to store responses based on the `/robots.txt` file name.
6. Add an encoded dot-segment and arbitrary directory before `/robots.txt`. For example, `/aaa/..%2frobots.txt`.
7. Send the request. Notice that the `200` response is cached. This shows that the cache normalizes the path to `/robots.txt`.

### Exploit the vulnerability to find the administrator's CSRF token
1. Use the `?` delimiter to attempt to construct an exploit as follows: `/my-account?%2f%2e%2e%2frobots.txt`. Send the request. Notice that this receives a `200` response, but doesn't contain evidence of caching.
2. Repeat this test using the `;` delimiter instead of `?`. Notice that this receives a `200` response with your API key and the `X-Cache: miss` header. Resend and notice that this updates to `X-Cache: hit`. This indicates that the cache normalized the path to `/robots.txt` and cached the response. You can use this payload for an exploit.
3. In the Body section, craft an exploit that will navigate the victim user to the malicious URL you crafted.
   ```sh
   <script>document.location="https://YOUR-LAB-ID.web-security-academy.net/my-account;%2f%2e%2e%2frobots.txt?wcd"</script>
   ```
4. Click Deliver exploit to victim.
5. Go to the URL that you delivered to the victim in your exploit.
6. Notice that in browser this redirects to the account login page. This may be because the browser redirects requests with invalid session data. Attempt the exploit in Burp instead.
7. Go to the Repeater tab that contains the `/my-account` request. Change the path to reflect the URL that you delivered to the victim in your exploit.
8. Notice that the response includes the CSRF token for the `administrator` user. Copy this.

### Craft an exploit
1. Send the POST `/my-account/change-email` request to Repeater.
2. In Repeater, replace the CSRF token with the administrator's token.
3. Change the email address in your exploit so that it doesn't match your own.
4. Generate a **CSRF PoC** using the engagement tools and paste it into the Body section.
5. Click Deliver exploit to victim to solve the lab.

## 📖 Key Takeaways
- Exact-Match Cache rules can be dangerous.
- Cache and Origin Normalization Must Match.
- Path delimiters create routing confusion.
- Cache deception can enable account takeover primitives.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7633a0bf-fe7d-4228-a296-b39e1f1bcef1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a4c5d376-6c0d-4cc9-b781-a97572d1d626" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5b7dbf08-ec58-4151-97c2-9fcd2e7a76bc" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d63047f2-3990-4ab8-93ee-d48c62e4a5a4" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ee9fab66-db76-4182-8850-62812664888c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/01d08b8a-4d34-4928-a297-6f70e32c8ea9" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/dcba6ca1-0a6c-4e4e-ade6-a1cde1186010" />
