# SQL Injection - Determining the number of columns returned by the query

## 📌 Lab Details
- **Title**: Using union attack to determine the number of columns.
- **Difficulty**: Medium
- **Category**: SQL Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/c0cf223d86b3189ea471d7a4dfe43987f9306dcd549be77599964da1b517b307?referrer=%2fweb-security%2fsql-injection%2funion-attacks%2flab-determine-number-of-columns

## 🔍 Summary
This lab demonstrated how SQL Injecetion can be used to determine the number of columns in a database.

## 🛠 Steps to Solve
1. Modified the `category` parameter, giving it the value `'UNION+SELECT+NULL--`.
2. If error occurs then add an additional column `'UNION+SELECT+NULL,NULL`.
3. Continued adding null values until error disappears.
   
## 📖 Key Takeaways
- Underestanding how SQL Queries can be used to manipulate databases.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/90590584-03b3-470a-ae1d-a709ead09e19)
![image](https://github.com/user-attachments/assets/3b8e0093-544d-4f2f-8200-c1ddc7eb2d12)

