#  Routing-based SSRF

## 📌 Lab Details
- **Title**: Routing-based SSRF
- **Difficulty**: Practitioner
- **Category**: HTTP Host header
- **Lab URL**: https://portswigger.net/academy/labs/launch/2744f28d772f25b5878c7fba870871916535bc5ae1458431fb62c5517346c62e?referrer=%2fweb-security%2fhost-header%2fexploiting%2flab-host-header-routing-based-ssrf

## 🔍 Summary
This lab demonstrates a **routing-based Server-Side Request Forgery (SSRF)** vulnerability caused by improper trust in the **Host** header.
The application uses the Host header when making backend requests. By supplying arbitrary values in the Host header, it is possible to force the server to send the requests to internal systems that are normally inaccessible from the internet.
The objective is to discover an internal admin panel hosted within the `192.168.0.0/24` network and use it to delete the user carlos.

## 🛠 Steps to Solve
1. Send the `GET /` request that received a `200` response to Burp Repeater.
2. In Burp Repeater, **Insert Collaborator payload** in place of the Host header value. Send the request.
3. Go to the Collaborator tab and click **Poll now**. You should see a couple of network interactions in the table, including an HTTP request. This confirms that you are able to make the website's middleware issue requests to an arbitrary server.
4. Use the **Burp Intruder** to find the IP address of the interanl admin panel: `Host: 192.168.0.§0§`.
5. When the attack finishes, click the **Status** column to sort the results. Notice that a single request received a `302` response redirecting you to `/admin`. Send this request to Burp Repeater.
6. In Burp Repeater, change the request line to `GET /admin` and send the request. In the response, observe that you have successfully accessed the admin panel.
7. Study the form for deleting users. Notice that it will generate a `POST` request to `/admin/delete` with both a CSRF token and `username` parameter. You need to manually craft an equivalent request to delete `carlos`.
8. Change the request method to `POST`. Change the path to `GET /admin/delete` and add the parameteres `csrf` and `username` in the request.
9. Send the request to delete `carlos` and solve the lab.

## 📖 Key Takeaways
- The Host header should never be trusted blindly.
- Backend services frequently use Host header values for routing decisions.
- SSRF can be triggered even without user-controlled URLs.
- Internal services often expose administrative functionality that is inaccessible externally.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/39292c2c-3aa5-4b4d-b325-2c63f6fc3700" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1135e323-43f0-4501-9195-60dee8ea2f2f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f948308d-e63a-48e4-a626-b9feb273ff40" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9f4cfba8-894e-45f1-b86b-242e8a05de9d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c4cadb21-6da4-46b6-8ae1-858aef307820" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ad3f3ee2-e7f1-4ddf-a9a8-1829cfdea2d4" />
