# Blind XXE - with out-of-band interaction via XML parameter entities

## 📌 Lab Details
- **Title**: Blind XXE with out-of-band interaction via XML parameter entities
- **Difficulty**: Practitioner
- **Category**: XML external entity injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/df91f530d50ed0f243fbe61ca82d8b7d7138ecbeb190a83d9047d5cd6b54ba78?referrer=%2fweb-security%2fxxe%2fblind%2flab-xxe-with-out-of-band-interaction-using-parameter-entities

## 🔍 Summary
This lab has an XXE vulnerability, but it **blocks normal (general) external entities** and doesn't show any output. So, to detect the vulnerability, you use a **parameter entity** (which is used in the DTD) to make the server **send a request to your Burp Collaborator subdomain**. If the server makes the request, the lab is solved.

## 🛠 Steps to Solve
1. Intercept the resulting **POST** request of "Check stock" in **Burp Suite**.

## 📖 Key Takeaways
- **General entities** like `<!ENTITY xxe SYSTEM "...">` are blocked in this lab.
- **Parameter entities** still work, so we use `%xxe;` in the DTD.
- Out-of-band (OAST) detection is used — you check for DNS/HTTP requests in **Burp Collaborator**.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/27c93340-cf3d-46f9-81f4-3544db98c464" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6374f649-bb30-452d-b34c-f1132cd481eb" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9cba6d24-7f6e-4bb5-8330-d7e1ab18ab63" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ed5efe4c-3763-47c7-bfa2-0030c2d806b1" />
