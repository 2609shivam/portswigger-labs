# CSRF - where token is duplicated in cookie

## 📌 Lab Details
- **Title**: CSRF where token is duplicated in cookie
- **Difficulty**: Practitioner
- **Category**: Cross-site Request Forgery
- **Lab URL**: https://portswigger.net/academy/labs/launch/681e7cf13b94031af2af240c4c07e7885859b8205e31bd6b511abb2077bc0227?referrer=%2fweb-security%2fcsrf%2fbypassing-token-validation%2flab-token-duplicated-in-cookie

## 🔍 Summary
This lab demonstrates a CSRF vulnerability in a site that uses the **"double submit cookie"** technique — where the CSRF token is placed both in a cookie and in a request body parameter, and the server simply compares the two.

## 🛠 Steps to Solve
1. Submit the email change form and capture the request.
2. Send the request to Burp Repeater and observe that the value of the `csrf` body parameter is simply being validated by comparing it with the `csrf` cookie.
3. Intercept the request in the lab's **search** bar to observe that the `search term` gets reflected in the Set-cookie header.
4. Perform a `header` injection in the search bar: `/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None`.
5. Create a **CSRF PoC** using the `csrf` token.
6. In order to auto submit the form, use the payload: `<img src="https://YOUR-LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None" onerror="document.forms[0].submit();"/>`.

## 📖 Key Takeaways
- CSRF protection fails if the attacker can set the same CSRF token in both cookie and body.
- If user input is reflected into a `Set-Cookie` header, an attacker can set arbitrary cookies.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/596c3b58-d657-46e2-af70-d814498d2c48)
![image](https://github.com/user-attachments/assets/6c055c72-5365-4158-8f28-256d06983208)
![image](https://github.com/user-attachments/assets/484d9061-a91b-4e6c-bbe7-fb11d18c2138)
![image](https://github.com/user-attachments/assets/f20b4d7a-e6c2-4604-8800-1d4b8956e85c)
![image](https://github.com/user-attachments/assets/23ca1f9c-cc73-481d-b9b5-283755403bed)
![image](https://github.com/user-attachments/assets/f6ffd4ad-a667-4f3a-be34-61e96ca834bf)
![image](https://github.com/user-attachments/assets/894249aa-c20f-4fcf-b4c5-da00034b86b6)
