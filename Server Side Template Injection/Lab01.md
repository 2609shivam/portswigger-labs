# Basic server-side template injection

## 📌 Lab Details
- **Title**: Basic server-side template injection (ERB)
- **Difficulty**: Practitioner
- **Category**: Server Side Template Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/401f612b786a17fa5d79a4695034393111a37b23e3c9a729cdf23878231c53e2?referrer=%2fweb-security%2fserver-side-template-injection%2fexploiting%2flab-server-side-template-injection-basic

## 🔍 Summary
This lab is vulnerable to **Server-Side Template Injection (SSTI)** due to the unsafe construction of an **ERB (Embedded Ruby)** template. User-controlled input from the `message` parameter is rendered directly by the template engine, allowing arbitary Ruby code execution.

## 🛠 Steps to Solve
1. Notice that when you try to view more details about the first product, a `GET` request uses the `message` parameter to render `"Unfortunately this product is out of stock"` on the home page.
2. In the ERB documentation, discover that the syntax `<%= someExpression %>` is used to evaluate an expression and render the result on the page.
3. Use ERB template systax to create a test payload containing a mathematical operation, for example: `<%= 7*7 %>`.
4. URL-encode this payload and insert it as the value of the message parameter in the URL.
5. Load the URL in the browser. Notice that in place of the message, the result of your mathematical operation is rendered on the page, in this case, the number **49**. This indicates that we may have a server-side template injection vulnerability.
6. From the Ruby documentation, discover the `File.delete` method, which can be used to execute arbitrary operating system commands.
7. Construct a payload to delete Carlos's file as follows: `<%= File.delete('/home/carlos/morale.txt') %>`.
8. URL-encode your payload and insert it as the value of the message parameter.

## 📖 Key Takeaways
- SSTI occurs when user input is embedded directly into server-side templates.
- ERB templates execute Ruby code enclosed within: `<%= someExpression %>`.
- SSTI can often lead to Remote Code Execution (RCE).
- Always test template engines with simple arithmetic payloads first.
- Never render untrusted user input directly inside templates.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9d1c1d2b-c1de-4d9a-a87f-a83ad447176b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c1c49035-465b-4f6e-b5a4-c486bd77a341" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e2ac3252-2d7d-4ec9-8cf2-710062ba7c6a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/93bcaab2-3cea-44ba-b395-3a7fab3e39f5" />
