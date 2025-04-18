# XSS - Stored XSS into anchor href attribute with double quotes HTML-encoded

## 📌 Lab Details
- **Title**: Stored XSS into anchor href attribute with double quotes HTML-encoded
- **Difficulty**: Apprentice
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/837b80f7b6cb679ddb307a046af6df7c851399e3337726434ac2681cba79f639?referrer=%2fweb-security%2fcross-site-scripting%2fcontexts%2flab-href-attribute-double-quotes-html-encoded

## 🔍 Summary
This lab demonstrates a stored XSS vulnerability where the attack is injected into the "Website" field of a comment. When the comment author's name (a link) is clicked, the `javascript:alert(1)` payload executes.

## 🛠 Steps to Solve
1. Post a comment with a random alphanumeric string in the "Website" input, then use Burp Suite to intercept the request and send it to Burp Repeater.
2. Make a second request in the browser to view the post and use Burp Suite to intercept the request and send it to Burp Repeater.
3. Observe that the random string in the second Repeater tab has been reflected inside an anchor `href` attribute.
4. Injecting the payload in the website section: `javascript:alert(1)`.
5. Clicking the name above your comment triggers an alert which verifies the attack.

## 📖 Key Takeaways
- Injecting `javascript:` into an anchor tag’s `href` can trigger script execution when clicked.
- Always validate and sanitize input used in `href` attributes.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/ec6720ae-8350-4d81-99ce-6c46bbd5b824)
![image](https://github.com/user-attachments/assets/28545c3d-d85d-42bb-b990-fed8941f061c)
![image](https://github.com/user-attachments/assets/e10852de-8e22-44f3-86d1-7faa0c017569)
![image](https://github.com/user-attachments/assets/7761ab49-7406-43c7-9ea9-1d0987b2b00c)
![image](https://github.com/user-attachments/assets/ef82f023-5efe-4be7-a3ff-0ab8135d4e3a)
![image](https://github.com/user-attachments/assets/706eefbf-dab0-4c85-9d1f-e67d133669a3)
