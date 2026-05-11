#  Modifying Serialized Data Types

## 📌 Lab Details
- **Title**: Modifying Serialized Data Types
- **Difficulty**: Practitioner
- **Category**: Json Web Token
- **Lab URL**: https://portswigger.net/academy/labs/launch/445830f9c9a370d46e54ae646b92704ea142e1ac742174de313060662a9c9a96?referrer=%2fweb-security%2fdeserialization%2fexploiting%2flab-deserialization-modifying-serialized-data-types

## 🔍 Summary
This lab demonstrates an **authentication bypass vulnerability** caused by insecure handling of serialized PHP objects stored inside a session cookie. <br>
The application serializes user session data into a PHP object and stores it client-side. Because the server trusts the serialized object without integrity protection, an attacker can tamper with object properties.

## 🛠 Steps to Solve
1. Log in using the given credentials. Capture the request `GET /my-account` and examine the session cookie using the Inspector to reveal a serialized PHP object.
2. Use the Inspector Panel to modify the session cookie as follows:
   - Update the length of the `username` attribute to `13`.
   - Change the username to `administrator`.
   - Change the access token to the integer `0`.
   - Update the data type label for the access token by replacing `s` with `i`.
3. Send the request. Notice that the response now contains a link to the admin panel at `/admin`, indicating that you have successfully accessed the page as the `administrator` user.
4. Change the path of your request to `/admin` and resend it. Notice that the `/admin` page contains links to delete specific user accounts.
5. Change the path of your request to `/admin/delete?username=carlos` and send the request to solve the lab.

## 📖 Key Takeaways
- Never trust serialized objects from the client.
- PHP loose comparison can introduce dangerous logic flaws.
- Type juggling attacks are still common in legacy PHP applications.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/36d61d0a-bdc5-4a13-a0fd-c347f464fdd2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d4f0837f-5a11-4170-b681-5b75706013cd" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/13d3d3e3-4441-4b29-8ad7-522d38fde146" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/aaeed40e-414f-44bd-95b8-573b86caecf4" />
