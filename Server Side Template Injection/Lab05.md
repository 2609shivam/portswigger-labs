#  Server-side template injection in a sandboxed environment

## 📌 Lab Details
- **Title**: Server-side template injection in a sandboxed environment
- **Difficulty**: Expert
- **Category**: Server Side Template Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/c61a3223815a3affe2d0768f4eaba10bca9fd31203ab3a555f662ce3456ed8a8?referrer=%2fweb-security%2fserver-side-template-injection%2fexploiting%2flab-server-side-template-injection-in-a-sandboxed-environment

## 🔍 Summary
This lab is vulnerable to **Server-Side Template Injection (SSTI)** in the **FreeMarker** template engine. Although the application attempts to sandbox template execution, the restrictions are incomplete and can be bypassed.
By abusing Java object methods that remain accessible inside the sandbox, an attacker can traverse the Java class hierarchy and gain access to sensitive functionality. This allows arbitrary file access and disclosure of sensitive data stored on the server.

## 🛠 Steps to Solve
1. Log in and edit one of the product description templates. Notice that you have access to the `product` object.
2. Load the JavaDoc for the `Object` class to find methods that should be available on all objects. Confirm that you can execute `${object.getClass()}` using the `product` object.
3. Explore the documentation to find a sequence of method invocations that grant access to a class with a static method that lets you read a file, such as:
   ```sh
   ${product.getClass().getProtectionDomain().getCodeSource().getLocation().toURI().resolve('/home/carlos/my_password.txt').toURL().openStream().readAllBytes()?join(" ")}
   ```
4. Enter this payload in one of the templates and save. The output will contain the contents of the file as decimal ASCII code points.
5. Convert the returned bytes to ASCII.
6. Submit the solution to solve the lab.

## 📖 Key Takeaways
- FreeMarker templates can execute Java methods on exposed objects.
- Sandboxes are only effective when dangerous methods are completely restricted.
- `getClass()` is often the starting point for Java SSTI exploitation.
- Java object traversal can expose file-system access even without direct code execution.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/49a9f66e-7da3-4162-8ee7-f72f56626310" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9227e8f0-fd8c-403f-93f8-c1f832a05f45" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/208add1f-05e2-4c65-b945-7494b433ec88" />
