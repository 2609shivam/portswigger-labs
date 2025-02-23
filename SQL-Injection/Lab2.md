# SQL Injection - Allowing Login Bypass

## 📌 Lab Details
- **Title**: SQL Injection - Allowing Login Bypass
- **Difficulty**: Easy
- **Category**: SQL Injection

## Summary
This lab demonstrated how an SQL injection attack can be used to bypass login

## 🛠 Steps to Solve
1. Used Burp Suite to intercept and modify the login request.
2. Modified the username parameter, giving it the value: administrator'--

## 📖 Key Takeaways
- Understanding how SQL queries can be manipulated
- Using '-- to comment out the remaining query
