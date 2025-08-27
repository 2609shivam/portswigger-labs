#  HTTP/2 request smuggling via CRLF injection

## 📌 Lab Details
- **Title**: HTTP/2 request smuggling via CRLF injection
- **Difficulty**: Practitioner
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/7d452275ba41f1aa0fc50b97490ae64b428d6401c7f193516bd15466490d8e01?referrer=%2fweb-security%2frequest-smuggling%2fadvanced%2flab-request-smuggling-h2-request-smuggling-via-crlf-injection

## 🔍 Summary
This lab demonstrates how **CRLF injection in HTTP/2 headers** can be abused to bypass front-end validation and smuggle malicious requests to the back-end. By inserting `\r\nTransfer-Encoding: chunked` inside a header value, the request is interpreted differently by the HTTP/2 front-end and the HTTP/1.1 back-end.

## 🛠 Steps to Solve
1. In Burp's browser, use the lab's search function a couple of times and observe that the website records your recent search history. Send the most recent `POST /` request to Burp Repeater and remove your session cookie before resending the request. Notice that your search history is reset, confirming that it's tied to your session cookie.
2. Expand the Inspector's **Request Attributes** section and make sure the protocol is set to HTTP/2.
3. Using the Inspector, add an arbitrary header to the request. Append the sequence `\r\n` to the header's value, followed by the `Transfer-Encoding: chunked` header:
   ```sh
    Name: foo
    Value: bar\r\n
    Transfer-Encoding: chunked
   ```
4. In the body, attempt to smuggle an arbitary prefix as follows:
   ```sh
   0

   SMUGGLED
   ```
   Observe that every second request you send receives a **404** response, confirming that you have caused the back-end to append the subsequent request to the smuggled prefix.
5. Change the body of the request to the following:
   ```sh
   0

   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Cookie: session=YOUR-SESSION-COOKIE
   Content-Length: 900

   search=x
   ```
6. Send the request and then go to the **Home** page of the lab to find the `victim's` request.
7. In Burp Repeater, send a request for the home page using the stolen session cookie to solve the lab.

## 📖 Key Takeaways
- Front-end (H2) treats \r\n as harmless text inside a header.
- Back-end (H1.1) interprets it as a new header boundary.
- CRLF injection in headers allows bypassing front-end sanitization of dangerous headers like `Transfer-Encoding`.
- This attack is only possible because of **protocol translation (H2 → H1.1)** and the **mismatched handling of CRLF**.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d6123932-f824-479c-ab43-dc90d4ba63f0" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/00640655-c437-4f5f-89a2-1966d26ad870" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/550504a4-6171-4108-a023-bc0c7983d1f5" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d86910b6-5003-4b48-b4e1-70303aa6d25d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6a4bfe08-e188-49ec-84cd-58b0eb1dfa5e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/482991e1-3091-48fd-b44b-0d6258ee37ca" />
