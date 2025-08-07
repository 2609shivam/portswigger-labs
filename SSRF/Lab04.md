# SSRF - with filter bypass via open redirection vulnerability

## 📌 Lab Details
- **Title**: SSRF with filter bypass via open redirection vulnerability
- **Difficulty**: Practitioner
- **Category**: Server-side Request Forgery
- **Lab URL**: https://portswigger.net/academy/labs/launch/3bd839053db0960daf7046201a4b58936ba21cbb4a15840db8237690eb2c4be7?referrer=%2fweb-security%2fssrf%2flab-ssrf-filter-bypass-via-open-redirection

## 🔍 Summary
This lab demonstrates how to bypass SSRF protections using **an open redirect**. The application blocks direct access to external/internal IPs like `192.168.0.12`, but includes a vulnerable endpoint (`/product/nextProduct`) that reflects a `path` parameter into a redirect. This open redirect is used to trick the server into making an SSRF request to an internal admin interface and delete the user carlos.

## 🛠 Steps to Solve
1. Intercept the "Check stock" **POST** request in Burp Suite and send it to Burp Repeater.
2. Tampering with `stockApi` parameter makes the observation that it isn't possible to make the server issue the request directly to a different host.
3. Click "next product" and observe that the `path` parameter is placed into the Location header of a redirection response.
4. Inject this exploit using the open redirection vulnerability into the `stockApi` parameter:
   ```sh
   /product/nextProduct?path=http://192.168.0.12:8080/admin
   ```
5. Observe that the stock checker shows you the admin page.
6. Amend the path to delete the target user:
   ```sh
   /product/nextProduct?path=http://192.168.0.12:8080/admin/delete?username=carlos
   ```

## 📖 Key Takeaways
- **SSRF filters can be bypassed** by chaining with open redirects on the same domain.
- Open redirects allow **redirecting server-side logic to restricted destinations**.
- **Real-world implication**: combining minor vulnerabilities (like open redirects) can lead to critical attacks (like SSRF to internal systems).

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/86e1e110-9050-4a20-aa0a-be4b5b0620fb" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/49b97e72-7624-4f9c-9d05-e8ad02072555" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1e2e607a-eb0a-4224-8973-eaebc4d5b568" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/26ff1da8-5aea-4913-8166-f24332abbd6e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f999511f-aa55-440b-aa5a-1257e4200507" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/277184d4-9a9c-437b-809b-dfb7216e2ccb" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b82531f1-42e3-4429-899a-86d51ba8a164" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9fd7452a-8045-4c61-8f43-7d5f793f46da" />
