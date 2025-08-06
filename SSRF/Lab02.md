# Basic SSRF - against another back-end system

## 📌 Lab Details
- **Title**: Basic SSRF against another back-end system
- **Difficulty**: Apprentice
- **Category**: Server-side Request Forgery
- **Lab URL**: https://portswigger.net/academy/labs/launch/41b2fc5f623ce28dcc489f78c8f94ef2b7de75258c86cff17b68ecc861409328?referrer=%2fweb-security%2fssrf%2flab-basic-ssrf-against-backend-system

## 🔍 Summary
This lab demonstrates SSRF where the vulnerable `stockApi` parameter can be used to scan an internal IP range (`192.168.0.X`) on port `8080`. The goal is to discover an internal admin panel and send a request to delete user `carlos`.

## 🛠 Steps to Solve
1. Intercept the "Check stock" **POST** request in Burp Suite and send it to Burp Intruder.
2. Change the URL in the `stockApi` parameter to:
   ```sh
   http://192.168.0.x:8080/admin
   ```
   then highlight the final octet of the **IP address** and click **Add §**.
3. Change the Payload type to numbers and enter 1 to 255, and 1 in the **Step** box respectively. Start the attack.
4. Find a single entry with **status code** `200`, this represents that the request is correct.
5. Read the HTML to identify the URL to delete the target user:
   ```sh
   http://192.168.0./admin/delete?username=carlos
   ```
6. Submit this URL to complete the lab.
   
## 📖 Key Takeaways
- SSRF can be used to scan internal IP ranges and ports.
- Enumerating internal hosts is possible by injecting varying IPs into the SSRF payload. This simulates port scanning or service discovery from the server's point of view.
- **Real-world implication**: This lab mimics how attackers pivot inside internal networks, targeting services like admin panels, cloud metadata endpoints, or internal APIs.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5f3cee95-ff8a-4e4d-b5d5-8c3845ad01ed" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1eb85d02-125c-45f2-a5bb-5b816a534bd6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c8740ec3-dea2-4290-9db2-a5dff3fc559f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f288e30d-4d6c-4300-9654-a94a75a7f1f5" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/19b8da82-32c0-4081-8aee-a94e0762df69" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cac368b2-f52a-4c9e-b1fd-88ca9c97da2c" />
