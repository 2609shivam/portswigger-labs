# Basic SSRF - against the local server

## 📌 Lab Details
- **Title**: Basic SSRF against the local server
- **Difficulty**: Apprentice
- **Category**: Server-side Request Forgery
- **Lab URL**: https://portswigger.net/academy/labs/launch/c12feb39a2450b08c055e9e00a51dc9457d91c3b07b1c435d560f7a0a1cd959a?referrer=%2fweb-security%2fssrf%2flab-basic-ssrf-against-localhost

## 🔍 Summary
This lab demonstrates a basic **Server-Side Request Forgery (SSRF)** vulnerability. A feature in the web app (stock checker) makes server-side HTTP requests based on user input. By modifying the `stockApi` parameter, we can trick the server into making internal requests to `http://localhost`, which is normally inaccessible from the outside.

## 🛠 Steps to Solve
1. Browse to `/admin` and observe that you can't directly access the admin page.
2. Intercept the "Check stock" **POST** request in Burp Suite.
3. Change the URL in the `stockApi` parameter to:
   ```sh
   http://localhost/admin
   ```
4. Read the HTML to identify the URL to delete the target user:
   ```sh
   http://localhost/admin/delete?username=carlos
   ```
5. Submit this URL to complete the lab.

## 📖 Key Takeaways
- **SSRF** occurs when user input is used to make server-side requests, allowing attackers to target internal services.
- **stockApi Parameter**: This parameter is vulnerable; changing it to `http://localhost/admin` exposes the internal admin interface.
- **Real-world Relevance**: SSRF can be used to access cloud metadata, internal admin panels, and more.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/af8879ae-a277-4159-9141-a21c913ad360" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4640a696-5d24-4e88-a8a1-996210341c98" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/87408453-a375-4e89-a148-946da6033c2a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2f11e8ba-e7dc-495b-83e4-dee4869a76b8" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/91437d42-ae97-4d85-b50e-17885aa4e25d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ca1e98af-e3d3-4d0b-8705-66f1e84cdc93" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2ee0190e-a3e6-4cf3-b890-645989b7af52" />
