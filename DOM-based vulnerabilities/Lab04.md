# DOM - based open redirection

## 📌 Lab Details
- **Title**: DOM based open redirection
- **Difficulty**: Practitioner
- **Category**: DOM-based vulnerability
- **Lab URL**: https://portswigger.net/academy/labs/launch/67ab900d88bcd094478d14d157338de5bf87302c124c802219a8ef29a4a65e0f?referrer=%2fweb-security%2fdom-based%2fopen-redirection%2flab-dom-open-redirection

## 🔍 Summary
This lab demonstrates **DOM-based open redirection** by exploiting a flawed client-side URL extraction to redirect the victim to your exploit server when they click the “Back to Blog” link

## 🛠 Solution
The blog post page contains the following link, which returns to the home page of the blog: 
```sh
<a href='#' onclick='returnURL' = /url=https?:\/\/.+)/.exec(location); if(returnUrl)location.href = returnUrl[1];else location.href = "/"'>Back to Blog</a>
```
The url parameter contains an open redirection vulnerability that allows you to change where the "Back to Blog" link takes the user. To solve the lab, construct and visit the following URL, remembering to change the URL to contain your lab ID and your exploit server ID:
```sh
https://YOUR-LAB-ID.web-security-academy.net/post?postId=4&url=https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/
```

## 📖 Key Takeaways
- Shows how **unsanitized client-side URL handling** (`location.href = returnUrl[1]`) **enables open redirects**.
- Demonstrates a **DOM-based redirect using URL parameters and regex extraction**.
- Reinforces why user-controlled URLs should be **validated against an allowlist before redirection** to prevent phishing and redirection-based attacks.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/fab6540d-380d-42dd-b894-877a29368413)
![image](https://github.com/user-attachments/assets/a2ecf931-a524-4c50-bb34-2c61019e0bdf)
![image](https://github.com/user-attachments/assets/51f0b4ba-dee0-450f-b44f-4ef7688dc506)
![image](https://github.com/user-attachments/assets/f7bf128a-9877-4333-8484-931b6b55c6f1)
![image](https://github.com/user-attachments/assets/103dcfc5-a049-4d53-9938-a08299a309f8)
