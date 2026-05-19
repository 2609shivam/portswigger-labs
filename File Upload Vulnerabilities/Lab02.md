#  Web Shell Upload via Extension Blacklist Bypass

## 📌 Lab Details
- **Title**: Web Shell Upload via Extension Blacklist Bypass
- **Difficulty**: Practitioner
- **Category**: Insecure Deserialization
- **Lab URL**: https://portswigger.net/academy/labs/launch/23b377e90a4c2eb083ef9f02cb4800e763796995a1449f85714790d15c30b9fa?referrer=%2fweb-security%2ffile-upload%2flab-file-upload-web-shell-upload-via-extension-blacklist-bypass

## 🔍 Summary
This lab demonstrates how an insecure file upload blacklist can be bypassed using a malicious `.htaccess` file.
The application blocks dangerous extensions like `.php`, but the blacklist fails to account for Apache server configuration files. By uploading a custom `.htaccess` file, it becomes possible to map a non-blacklisted extension (such as `.l33t`) to the PHP MIME type.

## 🛠 Steps to Solve
1. Log in and upload your image as your avatar.
2. Send the `GET` request `/files/avatars/<IMAGE>` to Burp Repeater.
3. Create an **exploit.php** file containing the script:
   ```sh
   <? php file_get_contents('/home/carlos/secret'); ?>
   ```
4. On attempting to upload the file you will observe that you are not allowed to upload files with a `.php` extension.
5. In Burp's proxy history, find the `POST /my-account/avatar` request that was used to submit the file upload. In the response, notice that the headers reveal that you're talking to an Apache server. Send this request to Burp Repeater.
6. In the `POST /my-account/avatar` request, make the following changes:
   - Change the value of the `filename` parameter to `.htaccess`.
   - Change the value of the `Content-Type` header to `text/plain`.
   - Replace the contents of the file (your PHP payload) with the following Apache directive:
     ```sh
     AddType application/x-httpd-php .l33t
     ```
7. Send the request and observe that the file was successfully uploaded.
8. Return to the original request for uploading your PHP exploit.
9. Change the value of the `filename` parameter from `exploit.php` to `exploit.l33t`. Send the request again and notice that the file was uploaded successfully.
10. Send the `GET` request `/files/avatars/exploit.l33t` to find the secret.

## 📖 Key Takeaways
- Blacklisting dangerous extensions is unreliable.
- Apache `.htaccess` files can modify server behavior if uploads are not restricted properly.
- User-controlled server configuration files are extremely dangerous.

## 🖼️ Screenshot
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e7e71e62-75fe-42c6-8289-3f45c3ab0947" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2e4a9c30-ca25-42e9-90f9-91019bf673b3" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1f296dc0-f932-46d1-ab42-d37ce3617b9e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/66729088-53dc-4368-b31f-5106009a85ea" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5dbc698d-a791-4788-bdda-10680081f83d" />
