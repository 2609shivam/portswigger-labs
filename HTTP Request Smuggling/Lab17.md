#  Bypassing access controls via HTTP/2 request tunnelling

## 📌 Lab Details
- **Title**: Bypassing access controls via HTTP/2 request tunnelling
- **Difficulty**: Expert
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/2c96a65b32a69545e9a98af5520a14bd4b397c70dd6b643c34c46352310b2f9b?referrer=%2fweb-security%2frequest-smuggling%2fadvanced%2frequest-tunnelling%2flab-request-smuggling-h2-bypass-access-controls-via-request-tunnelling

## 🔍 Summary
Use an **HTTP/2 CRLF injection + tunnelling** trick to make the front-end reveal its internal auth headers, then tunnel a forged `GET /admin` including those headers to the back-end. With a HEAD/length trick to force the front-end to over-read, you capture the admin response (or the delete URL) and remove **carlos**.

## 🛠 Steps to Solve
1. Send the `GET /` request to Burp Repeater. Expand the Inspector's **Request Attributes** section and make sure the protocol is set to HTTP/2.
2. Using the Inspector, append an arbitrary header to the end of the request and try smuggling a `Host` header in its name as follows:<br>
   **Name**
   ```sh
   foo: bar\r\n
   Host: abc
   ```
   **Value**
   ```sh
   xyz
   ```
3. Observe that the error response indicates that the server processes your injected host, confirming that the lab is vulnerable to CRLF injection via header names.
4. In the browser, notice that the lab's search function reflects your search query in the response. Send the most recent `GET /?search=YOUR-SEARCH-QUERY` request to Burp Repeater and upgrade it to an HTTP/2 request.
5. In Burp Repeater, right-click on the request and select **Change request method**. Send the request and notice that the search function still works when you send the `search` parameter in the body of a `POST` request.
6. Add an arbitrary header and use its name field to inject a large `Content-Length` header and an additional `search` parameter as follows:<br>
   **Name**
   ```sh
   foo: bar\r\n
   Content-Length: 100\r\n
   \r\n
   search=x
   ```
   **Value**
   ```sh
   xyz
   ```
7. In the main body of the request (in the message editor panel) append arbitrary characters to the original `search` parameter until the request is longer than the smuggled `Content-Length` header.
8. Send the request and observe that the response now reflects the headers that were appended to your request by the front-end server:
   ```sh
   0 search results for 'x: xyz
   Content-Length: 644
   cookie: session=YOUR-SESSION-COOKIE
   X-SSL-VERIFIED: 0
   X-SSL-CLIENT-CN: null
   X-FRONTEND-KEY: YOUR-UNIQUE-KEY
   ```
9. Change the request method to HEAD and edit your malicious header so that it smuggles a request for the admin panel. Include the three client authentication headers, making sure to update their values as follows:<br>
   **Name**
   ```sh
   foo: bar\r\n
   \r\n
   GET /admin HTTP/1.1\r\n
   X-SSL-VERIFIED: 1\r\n
   X-SSL-CLIENT-CN: administrator\r\n
   X-FRONTEND-KEY: YOUR-UNIQUE-KEY\r\n
   \r\n
   ```
   **Value**
   ```sh
   xyz
   ```
10. Send the request and observe that you receive an error response saying that not enough bytes were received. This is because the `Content-Length` of the requested resource is longer than the tunnelled response you're trying to read.
11. Change the `:path` pseudo-header so that it points to an endpoint that returns a shorter resource. In this case, you can use `/login`.
12. In the response, find the URL for deleting `carlos` (/admin/delete?username=carlos), then update the path in your tunnelled request accordingly and resend it. Although you will likely encounter an **error response**, carlos is deleted and the lab is solved.
    
## 📖 Key Takeaways
- H2 → H1 downgrade + `\r\n` in headers lets you inject/split requests.
- **High impact**: forging front-end authentication headers lets you access admin endpoints (full takeover) if back-end blindly trusts those headers.
- **Defenses**: normalize and sanitize H2→H1 downgrades (reject CRLF in header names/values), canonicalize/strip front-end injected auth headers at the proxy boundary, and enforce back-end per-request auth (don’t trust proxy-supplied headers).

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/97e51d63-6fbd-4648-abb9-5481e71e7098" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fd9d9420-e3b4-4bb5-97d5-8ad6be7be99e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/45191fea-59a1-4396-93d5-37d6a53f4c81" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/98056214-acc7-4365-9e86-75293f91fc61" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cec13159-6601-4e2c-a4ea-fcc60c016dab" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6ca4a3a8-7326-4954-87fe-0ae26536f26b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8308f18c-ef7d-4368-bc69-7389177c4f81" />
