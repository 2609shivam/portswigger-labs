#  Method-based access control can be circumvented

## 📌 Lab Details
- **Title**: Method-based access control can be circumvented
- **Difficulty**: Practitioner
- **Category**: Access-Control
- **Lab URL**: https://portswigger.net/academy/labs/launch/e115e4b43d59771bb787573f305861a795fb30f2447cef9b53618f4232d1c83b?referrer=%2fweb-security%2faccess-control%2flab-method-based-access-control-can-be-circumvented

## 🔍 Summary
The app relied on the HTTP method to allow admin actions. As a non-admin, changing the request method (e.g., POST → POSTX → GET) bypassed the check and let you promote yourself to administrator.

## 🛠 Steps to Solve
1. Log in using the admin credentials.
2. Browse to the admin panel, promote **carlos**, and send the HTTP request to Burp Repeater.
3. Open a private/incognito browser window, and log in with the non-admin credentials.
4. Attempt to re-promote **carlos** with the non-admin user by copying that user's session cookie into the existing Burp Repeater request, and observe that the response says "Unauthorized".
5. Change the method from `POST` to `POSTX` and observe that the response changes to "missing parameter".
6. Convert the request to use the `GET` method by right-clicking and selecting "Change request method".
7. Change the username parameter to your username and resend the request.

## 📖 Key Takeaways
- Enforce authorization checks on the server for every privileged endpoint.
- Disable or carefully validate method-override functionality (e.g., `X-HTTP-Method-Override`) and only accept it from trusted internal components.
- Normalize request handling so behavior is identical regardless of the incoming method (or explicitly reject unsupported ones).

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f63db6db-dd0e-49a6-9390-17e6d49217c4" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/48c6e8b2-2139-45fe-8cb1-b50189296560" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d204eee8-ee73-4f76-a655-f9692d2fafe1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4081e372-bb4b-420f-9690-7da8486a20d9" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1e5e4471-c36d-4872-8d0b-f3056ceadc01" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/90a2dc08-2f4f-431b-a255-9baf5744237c" />
