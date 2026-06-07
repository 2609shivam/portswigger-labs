# Exploiting path delimiters for web cache deception

## 📌 Lab Details
- **Title**: Exploiting path delimiters for web cache deception
- **Difficulty**: Practitioner
- **Category**: Web Cache Deception
- **Lab URL**: https://portswigger.net/academy/labs/launch/0c221a151722d2c5972eecae320718fcccc5d98a38b52914a5099c3837335f2f?referrer=%2fweb-security%2fweb-cache-deception%2flab-wcd-exploiting-path-delimiters

## 🔍 Summary
This lab demonstrates a **Web Cache Deception** vulnerability caused by a discrepancy between how the **origin server** and the **cache** interpret path delimiter characters.<br>
The origin server treats the `;` character as a path delimiter and ignores everything after it when routing requests. However, the cache does not recognize `;` as a delimiter and instead treats the entire URL as a cacheable resource. <br>
As a result, an attacker can trick the cache into storing sensitive user-specific content (such as API keys) under a URL that appears to reference a static file.

## 🛠 Steps to Solve
### Identify a target endpoint
1. In Burp's browser, log in to the application using the credentials `wiener:peter`.
2. Notice that the response contains your API key.

### Identify path delimiters used by the origin server
1. Send the `GET /my-account` request to Repeater.
2. Go to the Repeater tab. Add an arbitrary segment to the path. For example, change the path to `/my-account/abc`.
3. Send the request. Notice the `404 Not Found` response with no evidence of caching. This indicates that the origin server doesn't abstract the path to `/my-account`.
4. Remove the arbitrary segment and add an arbitrary string to the original path. For example, change the path to `/my-accountabc`.
5. Send the request. Notice the `404 Not Found` response with no evidence that the response was cached.
6. Send the request to Burp **Intruder**.
7. Make a **Sniper attack** and add a payload position after `/my-account` as follows: `/my-account§§abc`
8. Add a list of characters that may be used as delimiters under Payload configuration.
9. Under **Payload encoding**, deselect **URL-encode these characters**.
10. Start the attack.
11. When the attack finishes, sort the results by **Status code**. Notice that the `;` and `?` characters receive a `200` response with your API key. All other characters receive the `404 Not Found` response. This indicates that the origin server uses `;` and `?` as path delimiters.

### Investigate path delimiter discrepancies 
1. Go to the **Repeater** tab that contains the `/my-accountabc` request.
2. Add the `?` character after `/my-account` and add a static file extension to the path. For example, update the path to `/my-account?abc.js`.
3. Send the request. Notice that the response doesn't contain evidence of caching. This may indicate that the cache also uses `?` as a path delimiter.
4. Repeat this test using the `;` character instead of `?`. Notice that the response contains the `X-Cache: miss` header.
5. Resend the request. Notice that the value of the `X-Cache` header changes to `hit`. This indicates that the cache doesn't use `;` as a path delimiter and has a cache rule based on the `.js` static extension. You can use this payload for an exploit.

### Craft an exploit
1. Go to exploit server. In the Body section, craft an exploit that navigates the victim user `carlos` to the malicious URL you crafted earlier:
   ```sh
   <script>document.location="https://YOUR-LAB-ID.web-security-academy.net/my-account;wcd.js"</script>
   ```
2. Deliver exploit to victim. When the victim views the exploit, the response they receive is stored in the cache.
3. o to the URL that you delivered to carlos. Notice that the response includes the API key for carlos. Copy this.
4. Submit the solution to solve the lab.

## 📖 Key Takeaways
- Web Cache Deception often relies on differences between cache and origin-server URL parsing.
- Static-file cache rules can be abused to store dynamic authenticated content.
- A cached response may contain sensitive information belonging to another user.
- Ensure the cache and origin server use identical URL parsing rules.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f350f026-f675-4b31-9008-97424c8d2a33" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3d3b88f6-bfa1-43c9-a89c-38278257052f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ad62ec3a-45dd-4ad4-beb1-7038f2423f8d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/81574612-ed6d-4a49-87e4-2452b8a8a3ad" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1692b031-d8e3-44b0-bbc0-b4429762b242" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bd7cc68a-ed3b-4f9b-b5be-a369ccd2a535" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/07f4c23c-4fd7-4c62-9d83-e7e08ea789a8" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9c9ba95e-dd20-4daf-903e-9cd22aecd8ce" />
