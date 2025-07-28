# XXE - using external entities to retrieve files

## 📌 Lab Details
- **Title**: Exploiting XXE using external entities to retrieve files
- **Difficulty**: Apprentice
- **Category**: XML external entity injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/96a4274d6672c286187a77cc2a99f9342b37c04786b8c9e88a91bd9759d3e7d4?referrer=%2fweb-security%2fxxe%2flab-exploiting-xxe-to-retrieve-files

## 🔍 Summary
This lab contains a **XXE** vulnerability in the "Check stock" feature that accepts and parses XML data. The goal is to inject a malicious XML payload that return the contents of the `/etc/passwd` file.

## 🛠 Steps to Solve
1. Intercept the resulting **POST** request of "Check stock" in **Burp Suite**.
2. Insert the following **external entity** in between XML declaration:
   ```sh
   <!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]
   ```
3. Replace the `productId` with a reference to the external entity `&xxe`.

## 📖 Key Takeaways
- **XXE (XML External Entity)** vulnerabilities occur when XML parsers process external entities defined in a DOCTYPE.
- You can use the `<!ENTITY>` directive to define a reference to a file or remote resource.
- The injected entity gets dereferenced when used in the XML body, leaking sensitive files like `/etc/passwd`.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7f40542b-a46f-4c76-8bf2-6189fe805aa8" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d62c8963-b13e-45aa-a400-d7402f385147" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/18b1420a-e331-44b1-a231-0ca5c795c9cc" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2796c5b0-d000-4e99-91db-4e28f98c31af" />
