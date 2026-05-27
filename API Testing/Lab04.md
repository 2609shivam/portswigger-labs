# Exploiting Server-Side Parameter Pollution in a REST URL

## 📌 Lab Details
- **Title**: Exploiting Server-Side Parameter Pollution in a REST URL
- **Difficulty**: Expert
- **Category**: API Testing
- **Lab URL**: https://portswigger.net/academy/labs/launch/4967cb9ad484265a5742afb7b567c2732e03c1753ec5232085b0d0fc48a426f5?referrer=%2fweb-security%2fapi-testing%2fserver-side-parameter-pollution%2flab-exploiting-server-side-parameter-pollution-in-rest-url

## 🔍 Summary
The application's password reset functionality is vulnerable to **server-side parameter pollution (SSSP)** within a REST-style URL path.
User-controlled input from the `username` parameter is embedded directly into a backend API path without proper sanitization or normalization.

## 🛠 Steps to Solve
### Study the behavior
1. Trigger a password reset for the `administrator` user.
2. In Proxy > HTTP history, notice the `POST /forgot-password` request and the related `/static/js/forgotPassword.js` JavaScript file.
3. Send the `POST /forgot-password` request to **Burp Repeater**.
4. In the **Repeater** tab, resend the request to confirm that the response is consistent.
5. Send a variety of requests with a modified username parameter value to determine whether the input is placed in the URL path of a server-side request without escaping:
   1. Submit URL-encoded `administrator#` as the value of the username parameter.
      Notice that this returns an `Invalid route` error message. This suggests that the server may have placed the input in the path of a server-side request, and that the fragment has truncated some trailing data. Observe that the message also refers to an API definition.
   2. Change the value of the username parameter from `administrator%23` to URL-encoded `administrator?`, then send the request.
      Notice that this also returns an `Invalid route` error message. This suggests that the input may be placed in a URL path, as the ? character indicates the start of the query string and therefore truncates the URL path.
   3. Change the value of the username parameter from `administrator%3F` to `./administrator` then send the request.
      Notice that this returns the original response. This suggests that the request may have accessed the same URL path as the original request. This further indicates that the input may be placed in the URL path.
   4. Change the value of the username parameter from `./administrator` to `../administrator`, then send the request.
      Notice that this returns an `Invalid route` error message. This suggests that the request may have accessed an invalid URL path.

### Navigate to the API definition
1. Change the value of the username parameter from `../administrator` to `../%23`. Notice the `Invalid route` response.
2. Incrementally add further `../` sequences until you reach `../../../../%23` Notice that this returns a `Not found` response. This indicates that you've navigated outside the API root.
3. At this level, add some common API definition filenames to the URL path. For example, submit the following: `username=../../../../openapi.json%23`
   Notice that this returns an error message, which contains the following API endpoint for finding users:
   `/api/internal/v1/users/{username}/field/{field}`
   Notice that this endpoint indicates that the URL path includes a parameter called `field`.

### Exploit the vulnerability
1. Update the value of the `username` parameter, using the structure of the identified endpoint. Add an invalid value for the `field` parameter: `username=administrator/field/foo%23` . Send the request. Notice that this returns an error message, because the API only supports the email field.
2. Add `email` as the value of the `field` parameter: `username=administrator/field/email%23`
   Send the request. Notice that this returns the original response. This may indicate that the server-side application recognizes the injected `field` parameter and that `email` is a valid field type.
3. Review the `/static/js/forgotPassword.js` JavaScript file. Identify the password reset endpoint, which refers to the `passwordResetToken` parameter.
4. Change the value of the `field` parameter from `email` to `passwordResetToken`: `username=administrator/field/passwordResetToken%23`
   Send the request. Notice that this returns an error message, because the `passwordResetToken` parameter is not supported by the version of the API that is set by the application.
5. Using the `/api/` endpoint that you identified earlier, change the version of the API in the value of the `username` parameter: `username=../../v1/users/administrator/field/passwordResetToken%23`
   Send the request. Notice that this returns a password reset token.
6. In the Browser, send the request: `/forgot-password?passwordResetToken=12345678`
7. Set a new password.
8. Log in as administrator and delete the user carlos to solve the lab.

## 📖 Key Takeaways
- SSPP can occur in REST URL paths, not just query strings.
- Reserved URL characters can reveal backend routing behavior.
- Path traversal can manipulate internal API requests.
- OpenAPI files may leak sensitive internal endpoints.
- Error messages are extremely valuable during API exploitation.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/729eb4a7-b869-422a-89e2-3399ff40612c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4b093d1b-5913-4b65-b347-d10f0471300a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/722f61b2-7d3b-4eed-a368-a9c34d49241c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/232709fa-38b0-4d11-a637-e6204ed33978" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/437085c5-adeb-420d-8cd2-13acdfa0acf7" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/25671c91-7a90-4758-a329-cd7723fa02f9" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/019c6b45-ef4b-44a7-87c0-af9dface3b82" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5663a644-23a5-42e6-abb6-c486dbebead8" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/27d048a2-8de0-43e2-aa01-b1deb1ca80d7" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f943d2e7-8962-4c46-afdf-695a053628aa" />
