# Blind SSRF - with out-of-band detection

## 📌 Lab Details
- **Title**: Blind SSRF with out-of-band detection
- **Difficulty**: Practitioner
- **Category**: Server-side Request Forgery
- **Lab URL**: https://portswigger.net/academy/labs/launch/47869588c5b299d864f0e0fb23d97cb0ba5c5a686fcdc0de3a03637daf1ba3af?referrer=%2fweb-security%2fssrf%2fblind%2flab-out-of-band-detection

## 🔍 Summary
This lab demonstrates a **blind SSRF vulnerability** that doesn’t return any visible response to the attacker. The application’s analytics software makes a **server-side HTTP request to the Referer header URL** when a product page is loaded. By inserting a Burp Collaborator URL into the `Referer` header, we can detect the SSRF through **out-of-band DNS and HTTP interactions** in Burp Collaborator.

## 🛠 Steps to Solve
1. Visit a product, intercept the request in Burp Suite, and send it to **Burp Repeater**.
2. Change the **Referer header** in the request to your **Collaborator Payload**. Send the request.
3. Go to the **Collaborator** tab and click "Poll now".
4. You should see some **DNS** and **HTTP** interactions that were initiated by the application.

## 📖 Key Takeaways
- **Blind SSRF** means the server makes a request, but no direct response is shown to the attacker.
- The `Referer` header is **reflected in server-side analytics**, making it a useful injection point.
- Use **Burp Collaborator** to detect out-of-band interactions via DNS and HTTP.
- Look for **both DNS and HTTP requests** to confirm successful SSRF.
- This technique is useful when SSRF responses are **firewalled or delayed**.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0863e51d-9779-4c86-aa20-8681ea042dcc" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7a5bb31b-fe99-4f07-a1ea-78210fad55ec" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/545f0c76-6d0e-4267-8b8d-0fb864a1882d" />
