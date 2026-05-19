# Remote Code Execution via Polyglot Web Shell Upload  

## 📌 Lab Details
- **Title**: Remote Code Execution via Polyglot Web Shell Upload
- **Difficulty**: Practitioner
- **Category**: Insecure Deserialization
- **Lab URL**: https://portswigger.net/academy/labs/launch/328d640f4802f5285275092d76357ca4aa12c2b12fdd0fbfb606d7952c7114ec?referrer=%2fweb-security%2ffile-upload%2flab-file-upload-remote-code-execution-via-polyglot-web-shell-upload

## 🔍 Summary
The application allows users to upload avatar images and validates uploaded files by checking whether they are genuine image files.
However, the validation only verifies the image structure and does not prevent executable PHP code from being embedded inside image metadata.
By creating a **polyglot file** (a valid JPG image containing PHP code in EXIF metadata), an attacker can bypass the image validation and achieve **Remote Code Execution (RCE)** when the uploaded file is interpreted by the PHP engine

## 🛠 Steps to Solve
1. On your system, create a file called exploit.php containing a script for fetching the contents of Carlos's secret. For example:
   ```sh
   <? php file_get_contents('/home/carlos/secret'); ?>
   ```
2. Log in and attempt to upload the script as your avatar. Observe that the server successfully blocks you from uploading files that aren't images, even if you try using some of the techniques you've learned in previous labs.
3. In order to bypass the verification, Use an **image-PHP** polyglot, by adding header bytes in the existing `exploit.php`:
   ```sh
   GIF89a
   <? php file_get_contents('/home/carlos/secret'); ?>
   ```
4. In the browser, upload the polyglot image as your avatar, then go back to your account page.
5. Send the `GET` request `/files/avatars/exploit.php` to find the secret.

## 📖 Key Takeaways
- Image validation alone is insufficient for secure file uploads.
- Metadata inside images can contain executable code.
- Polyglot files can bypass content-based upload restrictions.
- Uploaded files should never be stored inside executable directories.
- Servers should strip EXIF metadata from uploaded images.

## 🖼️ Screenshot
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/03a7c3bd-377c-4880-9239-6d6ac0183771" />
<img width="1388" height="745" alt="image" src="https://github.com/user-attachments/assets/0e11612a-c33e-4acc-9796-e156c8724728" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8da14025-f7db-4358-92b9-f572f4bbd30b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f7f9c982-9df3-4b91-9c86-70c3579f017a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a1656e34-b899-460c-a2c9-61edd4418823" />
