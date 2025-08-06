# SSRF - with blacklist-based input filter

## 📌 Lab Details
- **Title**: SSRF with blacklist-based input filter
- **Difficulty**: Practitioner
- **Category**: Server-side Request Forgery
- **Lab URL**: https://portswigger.net/academy/labs/launch/f97f7df5a96da8563edc18e5544b8ef962f8ee47abd9b9bb3d30706f15bff341?referrer=%2fweb-security%2fssrf%2flab-ssrf-with-blacklist-filter

## 🔍 Summary
This lab demonstrates how **blacklist-based SSRF filters** can be bypassed. The `stockApi` parameter blocks direct access to `localhost` or `127.0.0.1`, and certain keywords like `/admin`. By using IP variations (`127.1`) and **double URL encoding**, you can bypass these filters.

## 🛠 Steps to Solve
1. Intercept the "Check stock" **POST** request in Burp Suite.
2. Change the URL in the `stockApi` parameter to `http://127.0.0.1/` and observer that the request is blocked.
3. Bypass the block by changing the URL to `http:127.1/`.
4. Change the URL to `http://127.1/admin` and observe that the URL is blocked again.
5. Obfuscate the **"a"** by double-URL encoding it to `%2561` to access the admin interface and delete the target user.

## 📖 Key Takeaways
- **Blacklists are weak SSRF defenses** — they can be bypassed using alternate IP formats or encodings.
- `127.1` is a valid loopback address and can bypass filters targeting `127.0.0.1`.
- **Double URL encoding** (e.g., `/admin` → `%2561dmin`) can defeat keyword filters.
- Always check for both **host-based** and **path-based** filtering.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b68e05cd-8074-4d6f-ab1a-617162a06e60" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/53772634-f830-4c62-8821-675677b35a58" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3b93a041-82cf-45d3-8f37-63c7fded7fdd" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a6782fd6-0f5b-426b-af02-72e9f6601cd5" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a02c839b-a6a3-4260-9d89-a0ccfea11b88" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/63b12918-7ccc-4339-a3a3-7edbd5fc20a2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c5c0eaf5-2949-4614-8aec-a570275b2456" />
