# Server-side template injection using documentation

## 📌 Lab Details
- **Title**: Server-side template injection using documentation
- **Difficulty**: Practitioner
- **Category**: Server Side Template Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/994a665c4ea7fa2804f3b34e32aff30f4d936a60c26b48a14db2fc1e4a83e04b?referrer=%2fweb-security%2fserver-side-template-injection%2fexploiting%2flab-server-side-template-injection-using-documentation

## 🔍 Summary
This lab contains a Server-Side Template Injection (SSTI) vulnerability in a product description template editor. The application uses the Freemarker template engine and allows user-supplied template expressions to be evaluated on the server.
By identifying the template engine through error messages and consulting the official Freemarker documentation, it is possible to discover a dangerous built-in function called `new()`. This function can instantiate arbitrary Java classes that implement the TemplateModel interface.

## 🛠 Steps to Solve
1. Log in and edit one of the product description templates. Enter expression such as `$(foobar)`, the error message in the output shows that the **freemarker** template engine is being used.
2. Study the Freemarker documentation and find that appendix contains an FAQs section with the question "Can I allow users to upload templates and what are the security implications?". The answer describes how the `new()` built-in can be dangerous.
3. Go to the "Built-in reference" section of the documentation and find the entry for `new()`. This entry further describes how `new()` is a security concern because it can be used to create arbitrary Java objects that implement the `TemplateModel` interface.
4. Load the JavaDoc for the `TemplateModel` class, and review the list of "All Known Implementing Classes".
5. Observe that there is a class called `Execute`, which can be used to execute arbitrary shell commands.
6. Now construct your own exploit and adapt it as follows:
   ```sh
   <#assign ex="freemarker.template.utility.Execute"?new()> ${ex("rm /home/carlos/morale.txt") }
   ```
7. Save the template and view the product page to solve the lab.

## 📖 Key Takeaways
- SSTI often begins with identifying the template engine through syntax or error messages.
- Official documentation can be enough to discover exploitation paths.
- Freemarker's new() built-in is extremely dangerous when exposed to untrusted users.
- Access to arbitrary Java classes can lead directly to Remote Code Execution (RCE).
- Documentation-driven exploitation is an important skill because public payloads may not always be available.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/15d6b116-5b59-4f1e-8df6-b610d2b38341" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1ae6daac-d4bc-450e-a0b6-83a459eaa243" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/031ce727-5859-4bb5-a069-6074b325da3b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f2b7ce5c-7c2b-4324-b126-fd130277f296" />
