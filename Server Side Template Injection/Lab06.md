#  Server-Side Template Injection with a Custom Exploit

## 📌 Lab Details
- **Title**: Server-Side Template Injection with a Custom Exploit
- **Difficulty**: Expert
- **Category**: Server Side Template Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/b6596a3779a729318ac89f63b0738e7abd4d39fde164ac01691f555eeb608396?referrer=%2fweb-security%2fserver-side-template-injection%2fexploiting%2flab-server-side-template-injection-with-a-custom-exploit

## 🔍 Summary
This lab contains a Server-Side Template Injection vulnerability in the preferred display name functionality. User-controlled input is evaluated as template code, providing access to the current `user` object.
By exploring application functionality and leveraging information disclosure from the avatar upload feature, it is possible to abuse methods exposed on the `user` object. Using these methods, an attacker can read arbitrary files and eventually trigger a file deletion operation.

## 🛠 Steps to Solve
1. While proxying traffic through Burp, log in and post a comment on one of the blogs.
2. Go to the "My account" page. Notice that the functionality for setting a preferred name is vulnerable to server-side template injection, as we saw in a previous lab. You should also have noticed that you have access to the `user` object.
3. Investigate the custom avatar functionality. Notice that when you upload an invalid image, the error message discloses a method called `user.setAvatar()`. Also take note of the file path `/home/carlos/User.php`. You will need this later.
4. Upload a valid image as your avatar and load the page containing your test comment.
5. In Burp Repeater, open the POST request for changing your preferred name and use the blog-post-author-display parameter to set an arbitrary file as your avatar:
   ```sh
   user.setAvatar('/etc/passwd')
   ```
6. Load the page containing your test comment to render the template. Notice that the error message indicates that you need to provide an image MIME type as the second argument. Provide this argument and view the comment again to refresh the template:
   ```sh
   user.setAvatar('/etc/passwd','image/jpeg')
   ```
7. To read the file, load the avatar using `GET /avatar?avatar=wiener`. This will return the contents of the `/etc/passwd` file, confirming that you have access to arbitrary files.
8. Repeat this process, to read the PHP file that you noted down earlier:
   ```sh
   user.setAvatar('/home/carlos/User.php','image/jpeg')
   ```
9. In the PHP file, Notice that you have access to the `gdprDelete()` function, which deletes the user's avatar. You can combine this knowledge to delete Carlos's file.
10. First set the target file as your avatar, then view the comment to execute the template:
    ```sh
    user.setAvatar('/home/carlos/.ssh/id_rsa','image/jpeg')
    ```
11. Invoke the `user.gdprDelete()` method and view your comment again to solve the lab.

## 📖 Key Takeaways
- SSTI is not limited to direct Remote Code Execution.
- Exposed objects often contain dangerous methods that can be abused.
- Arbitrary file read vulnerabilities frequently lead to further privilege escalation.
- Information disclosure can reveal application source files and internal functionality.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a4c00332-54c8-44fe-9633-c34fbe7d865d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/63338d6c-26f3-4234-9666-31b0736c9544" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/15148397-acd0-4b72-a0f6-4b79a3d5c05d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f5157102-3ca9-411a-929f-ba6485d93b1e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9cf6a7e7-05cc-4f51-a5d6-f150f19f5e05" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/75bdf391-de40-41b5-98c6-79849b255472" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5b946ec5-c2ec-4b94-9412-b06a37159168" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/af9d1783-5450-4793-8ef8-78bae46cc6ce" />
