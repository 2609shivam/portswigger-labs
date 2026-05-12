#  Using application functionality to exploit insecure deserialization

## 📌 Lab Details
- **Title**: Using application functionality to exploit insecure deserialization
- **Difficulty**: Practitioner
- **Category**: Insecure Deserialization
- **Lab URL**: https://portswigger.net/academy/labs/launch/5239aa2e1e68409057bbd4a72799714b50acabc542275a95bc242612a8c6ed41?referrer=%2fweb-security%2fdeserialization%2fexploiting%2flab-deserialization-using-application-functionality-to-exploit-insecure-deserialization

## 🔍 Summary
This lab demonstrates how insecure deserialization can become dangerous when serialized objects contain sensitive application functionality. <br>
The application stores session data inside a serialized PHP object within a client-controlled cookie. One of the object properties, `avatar_link`, is later used by the server during account deletion. Because the server trusts user-controlled serialized data without validation, an attacker can modify this property to point to arbitary files on the server.

## 🛠 Steps to Solve
1. Log in to your own account and notice the option to delete your account by sendina a `POST` request to `/my-account/delete`.
2. Capture the session cookie using Burp Repeater.
3. Edit the serialized data so that the `avatar_link` points to `/home/carlos/morale.txt`. Remember to update the length indicator.
4. Change the request to `POST /my-account/delete` and send the request. Your account will be deleted, along with Carlos' `morale.txt` file.

## 📖 Key Takeaways
- Serialized objects should never be trusted when controlled by users.
- Application functionality can unintentionally become an exploitation primitive.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4bfaf459-1fdd-47de-a8c4-4d6f427b94d5" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7c021dcb-8a4f-45c1-a44a-ac0d0533f5e1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/eaaaa1e8-ae40-46b5-be59-1deea6fec2a4" />
