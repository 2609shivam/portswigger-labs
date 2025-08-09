# SSRF - with whitelist-based input filter

## 📌 Lab Details
- **Title**: SSRF with whitelist-based input filter
- **Difficulty**: Expert
- **Category**: Server-side Request Forgery
- **Lab URL**: https://portswigger.net/academy/labs/launch/7e946e5cc74a1c4df272ba01df0415b53548e0dfdb326d35f1d58e9b38dcbda4?referrer=%2fweb-security%2fssrf%2flab-ssrf-with-whitelist-filter

## 🔍 Summary
This lab's stock check feature fetches data from an internal system. The stockApi parameter is validated against a whitelist (`stock.weliketoshop.net`). The goal is to bypass the filter to access the admin interface (`http://localhost/admin`) and delete the user **carlos**.

## 🛠 Steps to Solve
1. Intercept the "Stock check" **POST** request and send it to **Burp Repeater**.
2. Change the URL in the `stockApi` parameter to `http://127.0.0.1/` and observe that the application is parsing the URL, extracting the hostname, and validating it against a whitelist.
3. Change the URL to `http://username@stock.weliketoshop.net/` and observe that this is accepted, indicating that the URL parser supports embedded credentials.
4. Append a `#` to the username and observe that the URL is now rejected.
5. Double-URL encode the `#` to `%2523` and observe the extremely suspicious "Internal Server Error" response, indicating that the server may have attempted to connect to "username".
6. Finally, change the **URL** in the `stockApi` to:
   ```sh
   http://localhost:80%2523@stock.weliketoshop.net/admin/delete?username=carlos
   ```

## 📖 Key Takeaways
- Whitelist-based SSRF protection can be bypassed via URL parsing quirks (e.g., embedded credentials).
- Double URL encoding can trick poorly implemented parsers.
- Always test special characters (`@`, `#`, `%`) in URLs to bypass SSRF filters.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9c9fe1c8-5ff2-4528-8aaa-c000ac94ccd1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/518e599d-011f-4d07-bf62-42efe96cb44e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/66e7fe76-c574-4f21-a50f-e40c34afce9a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/900d18cd-3acc-4af9-b68d-6d7df5dffb65" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a57f76cb-975d-4ba9-b47f-028b29e56bc4" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/faf0767a-8aa1-4f6b-b8ca-8e51f2528d86" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5380f260-42dc-48f4-8905-17240a39eb87" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2feba92d-046c-4182-9972-3ee094d13dd1" />
