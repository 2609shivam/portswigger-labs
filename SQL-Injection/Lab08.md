# SQL Injection - UNION attack, Retrieving data from other tables

## 📌 Lab Details
- **Title**: Retrieving data from other tables.
- **Difficulty**: Practitioner
- **Category**: SQL Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/c94c990a42d1b1f39e53806b1d0d1ad51a43e1f2d5ec1a602c144d7817d3e91c?referrer=%2fweb-security%2fsql-injection%2funion-attacks%2flab-retrieve-data-from-other-tables

## 🔍 Summary
This lab demonstrated how to retrieve the username and password from a different table called users.

## 🛠 Steps to Solve
1. Injecting `'+UNION+SELECT+'abc','def'--` to determine the number of columns which contain text data.
2. Injecting this payload `'+UNION+SELECT+username,+password+FROM+users--` to retrieve the contents of the `users` table.
   
## 📖 Key Takeaways
- Using `UNION` command to retrieve username and password from other tables.
  
## 🖼️ Screenshot
![image](https://github.com/user-attachments/assets/c367950d-5996-42a0-9598-2197fd0de172)
![image](https://github.com/user-attachments/assets/08b4c0f8-42e2-4496-9934-3d775ba34cfb)
![image](https://github.com/user-attachments/assets/0ee622ce-312f-4e22-87df-a14c8b4d0581)
![image](https://github.com/user-attachments/assets/50e0c439-0599-40a2-a73f-7adc622b7547)
