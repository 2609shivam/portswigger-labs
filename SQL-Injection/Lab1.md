# SQL Injection - Retrieving Hidden Data

## 📌 Lab Details
- **Title**: SQL Injection - Retrieving Hidden Data
- **Difficulty**: Easy
- **Category**: SQL Injection
- **Lab URL**:https://portswigger.net/academy/labs/launch/d9a071d8264e85184722707ff5747bbbd77963967d1d01f1b22f5dd8252f9767?referrer=%2fweb-security%2fsql-injection%2flab-retrieve-hidden-data

## 🔍 Summary
This lab demonstrated how an SQL injection attack can be used to retrieve hidden data from a web application.

## 🛠 Steps to Solve
1. Injected `' OR '1'='1` into the search field.
2. Observed that all products were displayed, bypassing authentication.
3. Used `' UNION SELECT NULL, username, password FROM users--` to retrieve credentials.

## 📖 Key Takeaways
- Understanding how SQL queries can be manipulated.
- Using `UNION` to extract data from other tables.


