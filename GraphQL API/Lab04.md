# Performing CSRF Exploits Over GraphQL

## 📌 Lab Details
- **Title**: Performing CSRF Exploits Over GraphQL
- **Difficulty**: Practitoner
- **Category**: GraphQL API
- **Lab URL**: https://portswigger.net/academy/labs/launch/ee2c12983456a4312f8435c1ffb0292d55c10e35dd899de6fb8bb3560257ef75?referrer=%2fweb-security%2fgraphql%2flab-graphql-csrf-via-graphql-api

## 🔍 Summary
The application exposes GraphQL mutations through requests that accept the application/x-www-form-urlencoded content type.
Because the endpoint relies solely on session cookies for authentication and lacks CSRF protection mechanisms such as:
- CSRF tokens
- SameSite cookie protections
- Origin/Referer validation <br>
an attacker can force authenticated users to perform unintended GraphQL mutations via a malicious webpage. <br>
In this lab, the vulnerability allows an attacker to change another user's email address through a CSRF attack.

## 🛠 Steps to Solve
1. In the browser, access the lab and log in to your account.
2. Enter a new email address, then click Update email.
3. Send the **email-change** request to **Burp Repeater**.
4. In Repeater, amend the GraphQL query to change the email to a second different address.
5. In the response, notice that the email has changed again. This indicates that you can reuse a session cookie to send multiple requests.
6. Convert the request into a POST request with a `Content-Type` of `x-www-form-urlencoded`. To do this, send the request and select `Change request method` twice.
7. Notice that the mutation request body has been deleted. Add the request body back in with URL encoding. The body should look like below:
   ```sh
   query=%0A++++mutation+changeEmail%28%24input%3A+ChangeEmailInput%21%29+%7B%0A++++++++changeEmail%28input%3A+%24input%29+%7B%0A++++++++++++email%0A++++++++%7D%0A++++%7D%0A&operationName=changeEmail&variables=%7B%22input%22%3A%7B%22email%22%3A%22hacker%40hacker.com%22%7D%7D
   ```
8. Select **Engagement tools > Generate CSRF PoC**. Burp displays the **CSRF PoC** generator dialog.
9. Amend the HTML in the **CSRF PoC generator** dialog so that it changes the email a third time.
10. In the lab, click **Go to exploit server**.
11. Paste the HTML into the exploit server and click **Deliver exploit to victim** to solve the lab.

## 📖 Key Takeaways
- GraphQL endpoints are vulnerable to CSRF when they accept browser-friendly content types.
- `application/x-www-form-urlencoded` requests are especially dangerous because they can be sent through regular HTML forms.
- Authentication through cookies alone is insufficient protection.
- GraphQL mutations should implement proper CSRF defenses.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/929e0552-5234-493e-8f17-9f2e2d199ac8" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4d27f925-f713-4f4d-b0ba-48f782cce6ff" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7b08a2d0-bc8c-45e4-a8ce-4ddd4d619e9f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6b21fa87-785d-49ac-a897-5abf5d76f3d3" />
