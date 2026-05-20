#  Blind OS Command Injection with Output Redirection

## 📌 Lab Details
- **Title**: Blind OS Command Injection with Output Redirection
- **Difficulty**: Practitioner
- **Category**: OS Command Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/794c7ab29027c16b9a3fab84326c6f313a8af27a96f1833828edd38b26a4aca7?referrer=%2fweb-security%2fos-command-injection%2flab-blind-output-redirection

## 🔍 Summary
This lab demonstrates a **blind OS command injection** vulnerability where command output is not reflected in the HTTP response. The server has a writable directory: `/var/www/images/`. By redirecting command output into a file inside this directory, it becomes possible to retrieve the command results throught the application's image-loading functionality.

## 🛠 Steps to Solve
1. Use Burp Suite to intercept and modify the request that submits feedback.
2. Modify the `email` parameter, changing it to:
   ```sh
   email=x & whoami > /var/www/images/whoami.txt
   ```
3. Now use Burp Suite to intercept and modify the request that loads an image of a product.
4. Modify the `filename` parameter, changing the value to the name of the file you specified for the output of the injected command:
   ```sh
   filename=whoami.txt
   ```
5. Observe that the response contains the output from the injected command.

## 📖 Key Takeaways
- Blind OS command injection does not require direct command output.
- Output redirection can be used to write results into accessible files.
- Writable web directories are extremely dangerous when combined with command injection.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9e9967cb-783f-435b-8292-82efc349d650" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/85329de7-8df2-4290-bb15-b3939cb5dfeb" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b01c58cb-4fc1-4ba2-be0a-8e6b8c266536" />
