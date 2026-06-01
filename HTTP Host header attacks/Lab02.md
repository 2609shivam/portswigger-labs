#  SSRF via Flawed Request Parsing

## 📌 Lab Details
- **Title**: SSRF via Flawed Request Parsing
- **Difficulty**: Practitioner
- **Category**: HTTP Host header attacks
- **Lab URL**: https://portswigger.net/academy/labs/launch/fda358647ac3a4c7d8145cfc890d3191223eb52ffb099df6764d75a6e1c7d772?referrer=%2fweb-security%2fhost-header%2fexploiting%2flab-host-header-ssrf-via-flawed-request-parsing

## 🔍 Summary
This lab demonstrates a routing-based Server-Side Request Forgery (SSRF) vulnerability caused by flawed parsing of the request's intended destination. <br>
The application validates the `Host` header when a normal request is sent. However, when an absolute URL is supplied in the request line, the application validates the URL instead of the `Host` header. This creates an inconsistency where the backend middleware still uses the attacker-controlled `Host` header when making server-side requests. <br>
As a result, an attacker can force the server to communicate with internal systems that are normally inaccessible from the internet.

## 🛠 Steps to Solve
1. Send the `GET /` request that received a `200` response to Burp Repeater and study the lab's behavior. Observe that the website validates the Host header and blocks any requests in which it has been modified.
2. Observe that you can also access the home page by supplying an absolute URL in the request line as follows: `GET https://YOUR-LAB-ID.web-security-academy.net/`.
3. Notice that when you do this, modifying the Host header no longer causes your request to be blocked. Instead, you receive a timeout error. This suggests that the absolute URL is being validated instead of the Host header.
4. Use Burp Collaborator to confirm that you can make the website's middleware issue requests to an arbitrary server in this way.
   ```sh
   GET https://YOUR-LAB-ID.web-security-academy.net/
   Host: BURP-COLLABORATOR-SUBDOMAIN
   ```
5. Now, use **Burp Intruder** to scan the IP range `192.168.0.0/24` to identify the IP address of the admin interface. Send this request to Burp Repeater.
6. In Burp Repeater, append `/admin` to the absolute URL in the request line and send the request. Observe that you now have access to the admin panel, including a form for deleting users.
7. Construct a `POST` request and append `/admin/delete`, set parameter for `csrf` and `username`.
8. Set `username` as Carlos to delete the user and solve the lab.

## 📖 Key Takeaways
- Absolute URLs and `Host` headers may be processed differently by applications.
- Inconsistent parsing can create SSRF opportunities.
- Burp Collaborator is useful for confirming server-side requests.
- Internal services can often be discovered through SSRF-based network scanning.
- Never trust client-controlled routing information for backend requests.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b4253de7-5578-4512-9f5e-c5e969932fa3" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bf505119-d0f7-4ad9-8469-684743b5f874" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b343f017-f303-47b5-a623-ee78dc0b2af7" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/91536342-894d-46cd-9efe-41961844e985" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/37188b1e-9eef-40d2-a9fa-0b63ffc3f077" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f5383894-0cd3-4042-aef4-a0c29ca90312" />
