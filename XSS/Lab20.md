# XSS - Stored XSS into onclick event 

## 📌 Lab Details
- **Title**: Stored XSS into onclick event with angle brackets and double quotes HTML-encoded and single quotes and backslash escaped.
- **Difficulty**: Practitioner
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/53ddf9ef1abeee631133a34645f234b5a0115eb9e739c4cebbe2a4668735893b?referrer=%2fweb-security%2fcross-site-scripting%2fcontexts%2flab-onclick-event-angle-brackets-double-quotes-html-encoded-single-quotes-backslash-escaped

## 🔍 Summary
This lab contains a **stored XSS** vulnerability in the comment functionality. Inject a payload into the **comment** to trigger an `alert`.

## 🛠 Steps to Solve
1. Post a comment with a random alphanumeric string in the "Website" input and intercept using Burp Suite.
2. Make a second request to view the comment and intercept it to send to the repeater.
3. Observe that the random string in the second Repeater tab has been reflected inside an `onclick` event handler attribute.
4. Now modify your input to inject the payload: `http://foo?&apos;-alert(1)-&apos;`.
5. Verify the technique by visiting the URL again.

## 📖 Key Takeaways
- **Stored XSS** is dangerous - it affects everyone who views the page.
- If the user input ends up **inside a JavaScript context**, you can break out with `'`.
- Always check how input is reflected to craft the right payload.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/a343d765-81ad-47ea-ae3d-f398b72419df)
![image](https://github.com/user-attachments/assets/d70f9f29-05f3-4bb8-86af-685550795d26)
![image](https://github.com/user-attachments/assets/2c8eb853-50b7-4c92-b3ea-521876d6d354)
![image](https://github.com/user-attachments/assets/73f3d57d-d5ab-4252-82be-6e3cdf03665a)
![image](https://github.com/user-attachments/assets/154284b6-2447-44b9-9a39-ed2edf716136)
