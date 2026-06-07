# Exploiting origin server normalization for web cache deception

## 📌 Lab Details
- **Title**: Exploiting origin server normalization for web cache deception
- **Difficulty**: Practitioner
- **Category**: Web Cache Deception
- **Lab URL**: https://portswigger.net/academy/labs/launch/3c315ac6a411798fea7dd926a886c9943732dc14b1de47567bafce994f37d818?referrer=%2fweb-security%2fweb-cache-deception%2flab-wcd-exploiting-origin-server-normalization

## 🔍 Summary
This lab demonstrates a **Web Cache Deception** vulnerability caused by a discrepancy between how the **origin server** and the **cache** interpret URL paths.<br>
The origin server performs URL normalization by decoding and resolving path travel sequences such as: `/aaa/..%2fmy-account` which becomes: `/my-account`. <br>
However, the cache does **not** normalize the path in the same way. Instead, it applies its caching rules based on the raw URL path. <br>
Because the cache treats any URL beginning with `/resources/` as cacheable static content, an attacker can trick it into caching a dynamic account page containing another user's API key.

## 🛠 Steps to Solve
### Identify a target endpoint
1. In browser, log in to the application using the given credentials.
2. Notice that the response contains your API key.

### Investigate path delimiter discrepancies
You can refer to the previous lab for this.

### Investigate normalization discrepancies
1. Send a request with an arbitrary directory followed by an encoded dot-segment to the start of the original path. For example, `/aaa/..%2fmy-account`.
2. Send the request. Notice that this receives a `200` response with your API key. This indicates that the origin server decodes and resolves the dot-segment, interpreting the URL path as `/my-account`.
3. notice that the paths for static resources all start with the directory prefix `/resources`. Notice that responses to requests with the `/resources` prefix show evidence of caching.
4. Send an encoded dot-segment after the `/resources` path prefix, such as `/resources/..%2fYOUR-RESOURCE`.
5. Send the request. Notice that the `404` response contains the `X-Cache: miss` header.
6. Resend the request. Notice that the value of the `X-Cache` header changes to `hit`. This may indicate that the cache doesn't decode or resolve the dot-segment and has a cache rule based on the `/resources` prefix. To confirm this, you'll need to conduct further testing. It's still possible that the response is being cached due to a different cache rule.
7. Modify the URL path after `/resources` to a arbitrary string as follows: `/resources/aaa`. Send the request. Notice that the `404` response contains the `X-Cache: miss` header.
8. Resend the request. Notice that the value of the `X-Cache` header changes to `hit`. This confirms that there is a static directory cache rule based on the `/resources` prefix.

### Craft an exploit
1. Attempt to construct an exploit as follows: `/resources/..%2fmy-account`. Send the request. Notice that this receives a `200` response with your API key and the `X-Cache: miss` header.
2. Resend the request and notice that the value of the `X-Cache` header updates to `hit`.
3. In the Body section of the exploit server add the payload:
   ```sh
   <script>document.location="https://YOUR-LAB-ID.web-security-academy.net/resources/..%2fmy-account?wcd"</script>
   ```
4. Click Deliver exploit to victim. When the victim views the exploit, the response they receive is stored in the cache.
5. Go to the URL that you delivered to carlos in your exploit to find the API key.
6. Submit the api key to solve the lab.

## 📖 Key Takeaways
- Many origin servers automatically normalize paths. Attackers can abuse this behavior when caches do not perform the same normalization.
- Cache rules often depend on path prefixes.
- Cache and origin inconsistencies are dangerous.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/24d7a58b-0fe6-4e37-8944-629b93c6ccd1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/18179640-cfbf-44ad-9dcf-e6f5480c1496" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1ee11c3f-d5d0-408c-9873-d2b209f6fee4" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f5330f11-bc5d-44fa-9d8c-e818016e5612" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1eb7313e-5537-4f28-9705-6eb07d78dee5" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9785a591-ee29-42c8-a54a-996adb855652" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5a8b3acc-8c06-4757-8bcd-a16e88111235" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9a5713ca-f845-4891-a94a-8ca321cf5a92" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d496cee4-c9e2-4240-875e-180e6a48e70b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8eb6ff09-d5e4-4e49-84b5-96ac4313f6ac" />
