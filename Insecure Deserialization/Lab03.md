# Arbitrary object injection in PHP

## 📌 Lab Details
- **Title**: Arbitrary object injection in PHP
- **Difficulty**: Practitioner
- **Category**: Insecure Deserialization
- **Lab URL**: https://portswigger.net/academy/labs/launch/5ce272b4b2a84159f707a668e2cae9811a2f787c9eb75229368cc6fe2a7f53db?referrer=%2fweb-security%2fdeserialization%2fexploiting%2flab-deserialization-arbitrary-object-injection-in-php

## 🔍 Summary
This lab is vulnerable to **PHP insecure deserialization** through a client-controlled session cookie containing serialized objects. <br>
By accessing backup source code files, it is possible to identify a dangerous `__destruct()` magic method inside the `CustomTemplate` class. The destructor uses unlink on a user-controlled property, allowing arbitary file deletion.

## 🛠 Steps to Solve
1. Log in to your own account and notice the session cookie contains a serialized PHP object.
2. Notice that the website references the file `/libs/CustomTemplate.php`.
3. Append a tilde(`~`) at the end of the filename to read the source code.
4. In the source code, notice the `CustomTemplate` class contains the `__destruct()` magic method. This will invoke the `unlink()` method on the `lock_file_path` attribute, which will delete the file on this path.
5. Create a `CustomTemplate` object with the `lock_file_path` attribute set to `/home/carlos/morale.txt`. The final object:
   ```sh
   O:14:"CustomTemplate":1:{s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}
   ```
6. Base64 and URL-encode this object and save it to your clipboard.
7. Replace the session cookie with the modified one in your clipboard.
8. Send the request. The `__destruct()` magic method is automatically invoked and will delete Carlos's file.
   
## 📖 Key Takeaways
- Magic methods like:
  - __destruct()
  - __wakeup()
  - __toString()
  - __call()
- Backup files (`~`,`.bak`,`.old`, etc.) can expose sensitive source code.
- PHP object injection often becomes critical when combined with:
  - File operations
  - Command execution
  - SSRF
  - SQL queries

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/89449c9b-3a9b-497e-befb-2588b6ec0f97" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/50ebe9a2-32b6-48fe-a7b8-aa610faa1aa7" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/97638414-f43a-42d2-a79a-d67535126e23" />
