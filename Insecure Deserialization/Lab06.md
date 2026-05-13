#  Developing a custom gadget chain for PHP deserialization

## 📌 Lab Details
- **Title**: Developing a custom gadget chain for PHP deserialization
- **Difficulty**: Expert
- **Category**: Insecure Deserialization
- **Lab URL**: https://portswigger.net/academy/labs/launch/9b523aff587e849d868e17b9207adf2504c2a979abaa229b9e0dee29ccd47ba2?referrer=%2fweb-security%2fdeserialization%2fexploiting%2flab-deserialization-developing-a-custom-gadget-chain-for-php-deserialization

## 🔍 Summary
This lab is vulnerable to **PHP insecure deserialization** through a serialized session cookie. By analyzing the leaked PHP source code, we can build a custom gadget chain using magic methods:
- `__wakeup()`
- `__get()`

## 🛠 Steps to Solve
1. Log in to your own account and notice that the session cookie contains a serialized PHP object. Notice that the website references the file `/cgi-bin/libs/CustomTemplate.php`. Obtain the source code by submitting a request using the `.php~` backup file extension.
2. In the source code, notice that the `__wakeup()` magic method for a `CustomTemplate` will create a new `Product` by referencing the `default_desc_type` and `desc` from the `CustomTemplate`.
3. Also notice that the `DefaultMap` class has the `__get()` magic method, which will be invoked if you try to read an attribute that doesn't exist for this object. This magic method invokes `call_user_func()`, which will execute any function that is passed into it via the `DefaultMap->callback` attribute. The function will be executed on the `$name`, which is the non-existent attribute that was requested.
4. You can exploit this gadget chain to invoke `exec(rm /home/carlos/morale.txt)` by passing in a `CustomTemplate` object where:
   ```sh
   CustomTemplate->default_desc_type = "rm /home/carlos/morale.txt";
   CustomTemplate->desc = DefaultMap;
   DefaultMap->callback = "exec"
   ```
5. To solve the lab, Base64 and URL-encode the following serialized object, and pass it into the website via your session cookie:
   ```sh
   O:14:"CustomTemplate":2:{s:17:"default_desc_type";s:26:"rm /home/carlos/morale.txt";s:4:"desc";O:10:"DefaultMap":1:{s:8:"callback";s:4:"exec";}}
   ```

## 📖 Key Takeaways
- Never **unserialize** user controlled data.
- Magic methods can become dangerous gadgets.
- Dynamic property access can trigger unexpected execution.
- Source code disclosure can completely change exploitability.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5515bda2-2e4a-44d3-9b35-a5ca12b3deb7" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/58e41974-9b18-42f8-88fc-e71183790095" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f99985c7-abf8-47b6-b79c-852fd03064d9" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a876d87c-5e59-4b79-9e2b-64d2f89e29a5" />
