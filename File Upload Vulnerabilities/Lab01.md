#  Web shell upload via path traversal

## 📌 Lab Details
- **Title**: Web shell upload via path traversal 
- **Difficulty**: Practitioner
- **Category**: File Upload Vulnerabilities
- **Lab URL**: https://portswigger.net/academy/labs/launch/e39b4571e65ad58013bb6cf68240e2e7e59d087bfb1d58dd8673116f4bdf7135?referrer=%2fweb-security%2ffile-upload%2flab-file-upload-web-shell-upload-via-path-traversal

## 🔍 Summary
This lab demonstrates how an insecure file upload mechanism can be abused to achieve remote code execution (RCE). 
Although the application attempts to prevent execution of uploaded PHP files by storing them inside the `/files/avatars/` directory, the upload functionality is vulnerable to path traversal through the `filename` parameter in the multipart request.

## 🛠 Steps to Solve
1. Log in and upload the image as your avatar.
2. In Burp, go to **HTTP history**, notice the `GET` request to `/files/avatars/<IMAGE>`. Send this request to Burp Repeater.
3. On your system, create a file called `exploit.php`, containing a script for fetching the contents of Carlos's secret. For example:
   ```sh
   <?php echo file_get_contents('/home/carlos/secret'); ?>
   ```
4. Upload this script as your avatar. Notice that the website doesn't seem to prevent you from uploading PHP files.
5. Send the request `GET /files/avatars/exploit.php` and observe that instead of executing, the server just returned the contents of the file as plain text.
6. In Burp Repeater, go to the tab containing the `POST /my-account/avatar` request and find the part of the request body that relates to your PHP file. In the `Content-Disposition` header, change the `filename` to include a directory traversal sequence:
   ```sh
   Content-Disposition: form-data; name="avatar"; filename="../exploit.php"
   ```
7. Obfuscate the directory traversal sequence by URL encoding the forward slash (/) character, resulting in: `filename="..%2fexploit.php`.
8. Send the request and observe that the message now says `The file avatars/../exploit.php has been uploaded`. This indicates that the file name is being URL decoded by the server.
9. Send the request: `GET /files/avatars/../fexploit.php` to find the secret.
10. Submit the secret to solve the lab.

## 📖 Key Takeaways
- Blocking dangerous file extensions alone is insufficient.
- File upload paths must be securely validated.
- User-controlled filenames should never be trusted.
- Path traversal vulnerabilities can turn limited uploads into full RCE.

## 🖼️ Screenshot
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2dce3570-21a8-4ad7-b7af-e049a114978b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3895f5ae-1434-4b53-b92c-7f1f49f716f6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/300fe4f5-11e7-46d0-9e7a-565b473bf8a4" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9fc7fe6d-129a-460c-8326-758632ac8ffe" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1a41766f-f0f0-4d08-91b5-0e67728e006f" />
