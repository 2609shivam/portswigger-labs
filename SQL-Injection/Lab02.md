# SQL Injection - Allowing Login Bypass

## 📌 Lab Details
- **Title**: SQL Injection - Allowing Login Bypass
- **Difficulty**: Easy
- **Category**: SQL Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/befc0f5ddbc0e32a7adc4c9d4d286582f462f70bf32a8d5df15489d679dcd5c9?referrer=%2fweb-security%2fsql-injection%2flab-login-bypass

## Summary
This lab demonstrated how an SQL injection attack can be used to bypass login

## 🛠 Steps to Solve
1. Used Burp Suite to intercept and modify the login request.
2. Modified the username parameter, giving it the value: `administrator'--`

## 📖 Key Takeaways
- Understanding how SQL queries can be manipulated
- Using `'--` to comment out the remaining query
