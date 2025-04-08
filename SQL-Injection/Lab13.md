# SQL Injection - Blind SQLi with Time Delays

## 📌 Lab Details
- **Title**: Blind SQLi with time delays
- **Difficulty**: Practitoner
- **Category**: SQL INjection
- **Lab URL**: https://portswigger.net/academy/labs/launch/5d7c0f6ae7e03b08a47f4f6bff758a7c1b154753efea5dc4d4b847f7d1eab0a7?referrer=%2fweb-security%2fsql-injection%2fblind%2flab-time-delays

## 🔍 Summary
This lab demonstrated how SQL Injection can be used to produce time delays in a website response.

## 🛠 Steps to Solve
1. Used Burp Suite to intercept and modify the request containing the `TrackingId` cookie.
2. Modified the `TrackingId` cookie to `TrackingId=x'||pg_sleep(10)--`.
3. Submit the request and obsersve the 10 second delay.
   
## 📖 Key Takeaways
- Understanding the concept of `Time Delay`.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/025a078d-9ca4-4df0-b25c-4b85b1db4de6)
![image](https://github.com/user-attachments/assets/1129de6c-3443-45e9-8687-4aa5e10104a4)

