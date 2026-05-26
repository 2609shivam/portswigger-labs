#  Exploiting Server-Side Parameter Pollution in a Query String

## 📌 Lab Details
- **Title**: Exploiting Server-Side Parameter Pollution in a Query String
- **Difficulty**: Practitioner
- **Category**: API Testing
- **Lab URL**: https://portswigger.net/academy/labs/launch/d74fc3117a595f2183eb88a24360bb9c2a7472a910e4e97b4907971835d1f4e3?referrer=%2fweb-security%2fapi-testing%2fserver-side-parameter-pollution%2flab-exploiting-server-side-parameter-pollution-in-query-string

## 🔍 Summary
The application fails to safely encode user input before including it in a server-side API request.
This allows an attacker to:
1. Inject additional query parameters
2. Override existing parameters
3. Truncate the backend query string
4. Access sensitive internal fields such as `reset_token` <br>
Using this flaw, it becomes possible to retrieve the administrator password reset token, reset the password, and take over the account.

## 🛠 Steps to Solve
1. In Burp's browser, trigger a password reset for the `administrator` user.
2. In **Proxy > HTTP history**, notice the `POST /forgot-password` request and the related `/static/js/forgotPassword.js` JavaScript file.
3. Send the `POST /forgot-password` request to **Burp Repeater**.
4. In the **Repeater** tab, resend the request to confirm that the response is consistent.
5. Change the value of the `username` parameter from `administrator` to an invalid username, such as `administratorx`. Send the request. Notice that this results in an `Invalid username` error message.
6. Attempt to add a second parameter-value pair to the server-side request using a URL-encoded `&` character. For example, add URL-encoded `&x=y`:
`username=administrator%26x=y`
Send the request. Notice that this returns a `Parameter is not supported` error message. This suggests that the internal API may have interpreted `&x=y` as a separate parameter, instead of part of the username.
7. Attempt to truncate the server-side query string using a URL-encoded # character:
`username=administrator%23`
Send the request. Notice that this returns a `Field not specified` error message. This suggests that the server-side query may include an additional parameter called `field`, which has been removed by the `#` character.
8. Add a `field` parameter with an invalid value to the request. Truncate the query string after the added parameter-value pair. For example, add URL-encoded `&field=x#`:
`username=administrator%26field=x%23`
Send the request. Notice that this results in an `Invalid field` error message. This suggests that the server-side application may recognize the injected field parameter.
9. Brute-force the value of the `field` parameter using **Burp Intruder**. Use the **Server-side variable names** payload list, then start the attack. Notice that the requests with the username and email payloads both return a `200` response.
10. Change the value of the `field` parameter from `x#` to `email`:
`username=administrator%26field=email%23`
Send the request. Notice that this returns the original response. This suggests that `email` is a valid field type.
11. Review the `/static/js/forgotPassword.js` JavaScript file. Notice the password reset endpoint, which refers to the `reset_token` parameter:
`/forgot-password?reset_token=${resetToken}`
12. In the Repeater tab, change the value of the `field` parameter from `email` to `reset_token`:
`username=administrator%26field=reset_token%23`
Send the request. Notice that this returns a password reset token. Make a note of this.
13. In Burp's browser, enter the password reset endpoint in the address bar. Add your password reset token as the value of the `reset_token` parameter . For example:
`/forgot-password?reset_token=123456789`
14. Set a new password.
15. Log in as the `administrator` user using your password.
16. Go to the **Admin panel** and delete `carlos` to solve the lab.

## 📖 Key Takeaways
- URL-encoded special characters can manipulate backend-generated requests.
- Error messages often reveal internal API behavior.
- Query delimiters (`&`, `#`) are powerful in SSPP attacks.
- Hidden parameters can sometimes be brute-forced.
- JavaScript files frequently expose sensitive endpoint logic.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e091c0de-4bdf-4d3a-8bc6-03121db4f7b7" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7f365acd-065c-455d-bea5-b910af5b3d9d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2bca6af1-5eaa-40a2-85d6-a78a51b5c733" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/72ce6a07-7661-4d04-bf72-b2411d5a59ed" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4b56787d-c189-4787-a73c-6999a9d9ff8a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f9d73eb6-b258-44e3-b1cb-ce9b126cd244" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b9c94cb4-8bec-4f19-8b2e-d9462f8907e6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4a837c1f-f780-4cc0-86bd-f548e93e9d7e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6bf6bed8-48df-4c80-aee6-8380b9fa5b84" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/80caf8c3-0e51-420c-80d0-037fb999e582" />
