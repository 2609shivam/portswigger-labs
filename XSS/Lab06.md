# XSS - DOM XSS in jQuery selector sink using a hashchange event

## 📌 Lab Details
- **Title**: DOM XSS in jQuery selector sink using a hashchange event
- **Difficulty**: Apprentice
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/b73aad4efa87dfe5458720e5a4713f2b416825e550dc9d161f58635db7a48dcd?referrer=%2fweb-security%2fcross-site-scripting%2fdom-based%2flab-jquery-selector-hash-change-event

## 🔍 Summary
This lab exploits a DOM-based XSS vulnerability where jQuery uses `location.hash` unsafely in a selector. By injecting HTML via an iframe, we execute the `print()` function in the victim's browser.

## 🛠 Steps to Solve
1. Identifying the vulnerable `auto scroll` feature on the home page.
   It uses `$(location.hash)` in jQuery without sanitization.
2. Accessing the exploit server.
3. Crafting the exploit: `<iframe src="https://0a7300d3047e644983923cf00035004f.web-security-academy.net/#" onload="this.src +='<img src=1 onerror=print()>' " hidden="hidden"></iframe>`.
4. Click "View Exploit" to ensure `print()` is triggered in your browser.
5. Delivering the exploit to the victim.
   
## 📖 Key Takeaways
- `location.hash` is fully controllable by the attacker and should never be trusted without sanitization.
- jQuery's `$()` selector is vulnerable when used with attacker-controlled values like `location.hash`.
- This lab shows how even a simple hash-based scroll feature can become an XSS sink if not properly validated.
- Using `iframe` and `onload` to inject further content is a powerful XSS trick for bypassing basic protections.
  
## 🖼️ Screenshot
![image](https://github.com/user-attachments/assets/ef8da760-ccf8-400d-b9bb-d38d4d58e19c)
![image](https://github.com/user-attachments/assets/3531d908-b74a-444b-bfd8-705a964d9fa5)
![image](https://github.com/user-attachments/assets/6076f6a8-b5cc-4a75-81bf-d57a60ad8ec8)
![image](https://github.com/user-attachments/assets/d936b007-8a4a-4e90-bc00-11dfeb2377f2)
![image](https://github.com/user-attachments/assets/a7e467c2-679f-4e95-a200-2e7983a4ca48)
![image](https://github.com/user-attachments/assets/ddb2258a-5141-44b0-852b-4e3fb1e35e1f)
