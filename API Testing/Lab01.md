#  Finding and Exploiting an Unused API Endpoint

## 📌 Lab Details
- **Title**: Finding and Exploiting an Unused API Endpoint
- **Difficulty**: Practitioner
- **Category**: API Testing
- **Lab URL**: https://portswigger.net/academy/labs/launch/0784ab77e14f53e495ab0b8c4c39fc7008b9556c6d43961bcdba20bc43a58c3e?referrer=%2fweb-security%2fapi-testing%2flab-exploiting-unused-api-endpoint

## 🔍 Summary
The application exposes a hidden API endpoint that allows authenticated users to modify product prices using the `PATCH` HTTP method.
Although the frontend only uses the API to retrieve product prices with `GET` requests, the backend also supports additional functionality that was not intended to be publicly accessible.

## 🛠 Steps to Solve
1. Send the `/api/products/3/price` request to the Repeater.
2. Change the request method to `OPTIONS` reveals that `GET` and `PATCH` methods are allowed.
3. Change the method for the API request from `GET` to `PATCH`, notice that you recieve an `Unauthorized` message.
4. Log in the application using the given credentials.
5. Send the `/api/products/1/price` request to the Repeater.
6. Change the request method from `GET` to `PATCH`, then send the request. The error message specifies that the `Content-Type` should be `application/json`.
7. Use the **Content-Type Converter** extension to change the content-type to **json**.
8. Add a `price` parameter with a value of `0` to the JSON object `{"price":0}`. Send the request.
9. Reload the leather jacket product page. Notice that the price of the leather jacket is now $0.00.
10. Order the jacket to solve the lab.

## 📖 Key Takeaways
- APIs may expose hidden functionality through unused HTTP methods.
- `OPTIONS` requests can reveal dangerous backend capabilities.
- Improper authorization on API endpoints can lead to privilege escalation or business logic abuse.
- Error messages can help construct valid malicious requests.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e52bad6c-4c81-4188-b09c-ecf59856bb00" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/93cc13cf-c0a5-4794-a7dc-18d05b0bcbf1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a1541c7b-0001-474f-a081-4e01926b0d93" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/afa3f37d-b6e6-47f3-bb62-4a3a694fd62f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1dee713a-35de-4c57-80aa-f359857f4d7a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ce0b5428-6fae-409d-b33d-cc1986be7bb0" />
