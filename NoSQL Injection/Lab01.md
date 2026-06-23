# Exploiting NoSQL Injection to Extract Data

## 📌 Lab Details
- **Title**: Exploiting NoSQL Injection to Extract Data
- **Difficulty**: Practitioner
- **Category**: NoSQL Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/4a6884ba4c15a8785848b9e6bd721f6e7086c5ddfa9e3acce8d35f44effbca30?referrer=%2fweb-security%2fnosql-injection%2flab-nosql-injection-extract-data

## 🔍 Summary
This lab contains a user lookup feature backed by a MongoDB database. The application is vulnerable to **JavaScript-based NoSQL Injection**, allowing user-controlled input to be evaluated on the server. <br>
The objective is to extract the **administrator** user's password by leveraging boolean-based conditions and then log in as the administrator.

## 🛠 Steps to Solve
1. In the browser, access the lab and log in to the application using the credentials.
2. Send the `GET /user/lookup?user=wiener` request to Repeater.
3. In Repeater, submit a `'` character in the user parameter. Notice that this causes an error. This may indicate that the user input was not filtered or sanitized correctly.
4. Submit a valid JavaScript payload in the user parameter. For example, you could use `wiener'+'`. Make sure to URL-encode the payload. Notice that it retrieves the account details for the `wiener` user, which indicates that a form of server-side injection may be occurring.
5. Identify whether you can inject boolean conditions to change the response:
   1. Submit a false condition in the user parameter. For example: `wiener' && '1'=='2`. Make sure to URL-encode the payload. Notice that it retrieves the message `Could not find user`.
   2. Submit a true condition in the user parameter. For example: `wiener' && '1'=='1`. Notice that it no longer causes an error. Instead, it retrieves the account details for the `wiener` user. This demonstrates that you can trigger different responses for true and false conditions.
6. Identify the password length. Change the user parameter to `administrator' && this.password.length == §1§ || 'a'=='b` and use the Burp Intruder to brute force the result.
7. Use Intruder to enumerate the password using a **Cluster Bomb** attack. Change the user parameter to `administrator' && this.password[§0§]=='§a§ || '`.
8. In the browser, login to solve the lab.

## 📖 Key Takeaways
- MongoDB applications may evaluate user input as JavaScript if unsafe query construction is used.
- Boolean-based NoSQL Injection enables blind extraction of sensitive data.
- Response differences can be used to infer password length and characters.
- Never concatenate user input into MongoDB JavaScript expressions. Use parameterized queries and disable server-side JavaScript execution where possible.

## 🖼️ Screenshot 
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/04ee9bbb-e690-45d5-9ffd-7302059aa42b" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/fc52e0d4-a74a-4ed5-8e9c-0246d3d85931" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/3cb2d649-bff6-440c-95f7-51f74563a856" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/744c3c31-a3da-47a0-a9cc-2a06f85f3e39" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/322cd2b8-c16f-423a-924b-2a4bf2a147b5" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/584191af-3b58-4860-b9d2-8093c9f6178b" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/69045814-c009-4ac8-a8c5-fd86d8cce954" />
