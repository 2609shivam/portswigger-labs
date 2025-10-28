#  URL-based access control can be circumvented

## 📌 Lab Details
- **Title**: URL-based access control can be circumvented
- **Difficulty**: Practitioner
- **Category**: Access-Control
- **Lab URL**: https://portswigger.net/academy/labs/launch/9955f5350904edabe30089828d2c08ace886493264a0b469354503102379a3e9?referrer=%2fweb-security%2faccess-control%2flab-url-based-access-control-can-be-circumvented

## 🔍 Summary
A front-end blocker prevented direct browsing to /admin, but the back-end framework honored the `X-Original-URL` header. 

## 🛠 Steps to Solve
1. Try to load `/admin` and observe that you get blocked. Notice that the response is very plain, suggesting it may originate from a front-end system.
2. Send the request to Burp Repeater. Change the URL in the request line to `/` and add the HTTP header `X-Original-URL: /invalid`. Observe that the application returns a "not found" response. This indicates that the back-end system is processing the URL from the `X-Original-URL` header.
3. Change the value of the `X-Original-URL` header to `/admin`. Observe that you can now access the admin page.
4. To delete **carlos**, add `?username=carlos` to the real query string, and change the `X-Original-URL` path to `/admin/delete`.

## 📖 Key Takeaways
- **Security gates at the edge aren’t enough**. Blocking paths in a front-end or WAF does not protect the backend if the backend still performs routing/authorization based on untrusted headers.
- **Authorization must be enforced server-side**. The backend must check authentication/authorization for every sensitive endpoint regardless of what the front-end or proxy does.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7f1e172d-7432-444d-8d25-c8abfa46a4fb" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b85225f6-2ee3-4cdb-90b0-b75b1eb7125a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d51f70f8-afe0-4777-84c6-696bfe9aea6e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1176793e-1c1c-4593-bb12-63224e3a46d6" />
