# Exploiting cache server normalization for web cache deception

## 📌 Lab Details
- **Title**: Exploiting cache server normalization for web cache deception
- **Difficulty**: Practitioner
- **Category**: Web Cache Deception
- **Lab URL**: https://portswigger.net/academy/labs/launch/8154d34b0b2f71eb7f02a610af2cf34f8bd1bffb9583b7c5b75bbe6ab66c201b?referrer=%2fweb-security%2fweb-cache-deception%2flab-wcd-exploiting-cache-server-normalization

## 🔍 Summary
This lab demonstrates a Web Cache Deception vulnerability caused by a discrepancy between how the cache server and the origin server interpret URL paths. The cache normalizes encoded path traversal sequences (..%2f) and applies cache rules based on the resulting path, while the origin server does not. By combining this with a path delimiter understood only by the origin, an attacker can trick the cache into storing sensitive account data as if it were a cacheable static resource, allowing other users' data to be retrieved from the cache.

## 🛠 Steps to Solve
### Investigate path delimiters used by the origin server
1. Send the `GET /my-account` request to Burp Repeater.
2. Change the URL path to `/my-account/abc`, then send the request. Notice the `404` Not Found response. This indicates that the origin server doesn't abstract the path to `/my-account`.
3. Change the path to `/my-accountabc`, then send the request. Notice that this returns a 404 Not Found response with no evidence of caching.
4. Perform a sniper attack using Burp Intruder and add a payload position after `/my-account` as follows: `/my-account§§abc`.
5. Under Payload configuration, add a list of characters that may be used as delimiters.
6. When the attack finishes, sort the results by Status code. Notice that the `#`, `?`, `%23`, and `%3f` characters receive a 200 response with your API key. This indicates that they're used by the origin server as path delimiters. Ignore the `#` character. It can't be used for an exploit as the victim's browser will use it as a delimiter before forwarding the request to the cache.

### Investigate path delimiter discrepancies
1. Send a request to a static extension to the path: `/my-account?abc.js`
2. Send the request. Notice that the response doesn't contain evidence of caching. This either indicates that the cache also uses `?` as a path delimiter, or that the cache doesn't have a rule based on the `.js` extension.
3. Repeat this test using the `%23` and `%3f` characters instead of `?`. Notice that the responses don't show evidence of caching.

### Investigate normalization discrepancies
1. Remove the query string and add an arbitrary directory followed by an encoded dot-segment to the start of the original path. For example, `/aaa/..%2fmy-account`.
2. Send the request. Notice that this receives a `404` response. This indicates that the origin server doesn't decode or resolve the dot-segment to normalize the path to `/my-account`.
3. Notice that static resources share the URL path directory prefix `/resources`. Notice that responses to requests with the `/resources` prefix show evidence of caching.
4. Add an encoded dot-segment and arbitrary directory before the `/resources` prefix. For example, `/aaa/..%2fresources/YOUR-RESOURCE`.
5. Send the request. Notice that the `404` response contains the `X-Cache: miss` header.
6. Resend the request. Notice that the value of the `X-Cache` header updates to `hit`. This may indicate that the cache decodes and resolves the dot-segment and has a cache rule based on the `/resources` prefix.
7. Add an encoded dot-segment after the `/resources` path prefix as follows: `/resources/..%2fYOUR-RESOURCE`.
8. Send the request. Notice that the `404` response no longer contains evidence of caching. This indicates that the cache decodes and resolves the dot-segment and has a cache rule based on the `/resources` prefix.

### Craft an exploit
1. Use the `?` delimiter to attempt to construct an exploit as follows: `/my-account?%2f%2e%2e%2fresources`.
2. Send the request. Notice that this receives a `200` response with your API key, but doesn't contain evidence of caching.
3. Repeat this test using the `%23` and `%3f` characters instead of `?`. Notice that when you use the `%23` character this receives a `200` response with your API key and the `X-Cache: miss` header. Resend and notice that this updates to `X-Cache: hit`. You can use this delimiter for an exploit.
4. In the Body section, craft an exploit that navigates the victim user carlos to a malicious URL:
   ```sh
   <script>document.location="https://YOUR-LAB-ID.web-security-academy.net/my-account%23%2f%2e%2e%2fresources?wcd"</script>
   ```
5. Click Deliver exploit to victim.
6. Go to the URL that you delivered to `carlos` in your exploit.
7. Notice that the response includes the API key for the user carlos. Copy this.
8. Submit the API key to solve the lab.

## 📖 Key Takeaways
- Cache and origin servers may normalize URLs differently.
- Encoded dot-segments (`..%2f`) are commonly handled differently by caches and origins.
- Static directory cache rules are a valuable target when hunting for Web Cache Deception.
- Web Cache Deception often relies on making sensitive dynamic content appear to belong to a cacheable static resource.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5ed57d26-a76c-4be5-942e-4590030dbea1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ea313350-308a-4cb4-a471-76fdab16a263" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c0a1b859-8725-43c1-b007-d885b837c272" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ccc8c0e4-aecb-4f0d-b9e8-c6e4eb674724" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/02e1e288-b122-45eb-85f1-20e0cb63d666" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b3ca642c-50ed-4c39-82c3-e3aa08e92c8e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/22530699-9489-4442-822f-4f69d1f3636c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/22e2d4aa-95fa-4c19-9978-6cdbb5bb8c81" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/872d857b-a7a4-4c32-b2b7-816b8154646b" />
