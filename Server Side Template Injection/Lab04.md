#  Server-Side Template Injection with Information Disclosure via User-Supplied Objects

## 📌 Lab Details
- **Title**: Server-Side Template Injection with Information Disclosure via User-Supplied Objects
- **Difficulty**: Practitioner
- **Category**: Server Side Template Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/df068afc1423346dc3fc00c7af345df7970e4571cb93bd574dcce698e8bb50ed?referrer=%2fweb-security%2fserver-side-template-injection%2fexploiting%2flab-server-side-template-injection-with-information-disclosure-via-user-supplied-objects

## 🔍 Summary
The application allows priviledged users to edit product description templates. User-supplied template code is rendered on the server without proper restrictions, resulting in a Server-Side Template Injection (SSTI) vulnerability.
Unlike previous SSTI labs that focused on achieving Remote Code Execution (RCE), this lab demonstrates how SSTI can be used for **information disclosure**. By accessing internal template objects, an attacker can retrieve sensitive configuration values, including Django's `SECRET_KEY`.

## 🛠 Steps to Solve
1. Log in and edit one of the product description templates.
2. Change one of the template expressions to something invalid, such as a fuzz string `${{<%[%'"}}%\`, and save the template. The error message in the output hints that the Django framework is being used.
3. Study the Django documentation and notice that the built-in template tag `debug` can be called to display debugging information.
4. In the template, remove your invalid syntax and enter the following statement to invoke the `debug` built-in: `<% debug %>`.
5. Save the template. The output will contain a list of objects and properties to which you have access from within this template. Crucially, notice that you can access the `settings` object.
6. Study the `settings` object in the Django documentation and notice that it contains a `SECRET_KEY` property, which has dangerous security implications if known to an attacker.
7. In the template, remove the `{% debug %}` statement and enter the expression `{{settings.SECRET_KEY}}`.
8. Save the template to output the framework's secret key.
9. Submit the key to solve the lab.

## 📖 Key Takeaways
- SSTI can lead to information disclosure even without RCE.
- Django's `{% debug %}` tag is useful for enumerating accessible objects.
- Sensitive framework objects should never be exposed to user-controlled templates.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a82906af-1f0b-43a0-a934-8f043840b452" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/50d64453-bebb-45da-9a32-5b52fab71de0" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b6fc842a-1855-4971-aca4-c0447b7b4ff9" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b9a7c4b5-41a4-40b4-b562-cda97719fb02" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3aa90c80-ae19-444c-9a26-01926c273f68" />
