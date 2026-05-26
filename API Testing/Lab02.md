#  Exploiting a Mass Assignment Vulnerability

## 📌 Lab Details
- **Title**: Exploiting a Mass Assignment Vulnerability
- **Difficulty**: Practitioner
- **Category**: API Testing
- **Lab URL**: https://portswigger.net/academy/labs/launch/5d91c4a0e2767106aefce961db7e938299e9506731dc1a2de30bb258a82c697a?referrer=%2fweb-security%2fapi-testing%2flab-exploiting-mass-assignment-vulnerability

## 🔍 Summary
The checkout API exposes internal object properties through its JSON structure.
Although the frontend does not send the `chosen_discount` parameter in the checkout request, the backend still accepts and processes it.
Because the server fails to enforce a strict allowlist of accepted parameters, an attacker can inject hidden fields into the request body and manipulate business logic.

## 🛠 Steps to Solve
1. Log in the application using the given credentials.
2. Send both the `GET` and `POST` API requests to **Burp Repeater**.
3. Notice that the response to the `GET` request contains the same JSON structure as the `POST` request. Observe that the JSON structure in the `GET` response includes a `chosen_discount` parameter, which is not present in the `POST` request.
4. Add the `chosen_discount` parameter to the `POST` request.
   ```sh
   {
    "chosen_discount":{
        "percentage":0
    },
    "chosen_products":[
        {
            "product_id":"1",
            "quantity":1
        }
       ]
   }
   ```
5. Send the request. Notice that adding the `chosen_discount` parameter doesn't cause an error.
6. Change the `chosen_discount` value to the string `"x"`, then send the request. Observe that this results in an error message as the parameter value isn't a number. This may indicate that the user input is being processed.
7. Change the `chosen_discount` percentage to `100`, then send the request to solve the lab.

## 📖 Key Takeaways
- Hidden API parameters can often be discovered by comparing requests and responses.
- Mass assignment vulnerabilities commonly occur in modern JSON APIs.
- Accepting unexpected parameters can lead to privilege escalation or business logic abuse.
- Backend validation errors are useful indicators that hidden fields are processed internally.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/69bea5ff-4c2b-4bd5-976a-597ece06c9f3" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cbe380d7-3c49-466c-8802-fd41c5c13f0d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/73a59c27-b67f-47bd-bf7b-e2e240e3af8c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/75f0958e-7c1e-4aa5-a739-e1a1290665e0" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/b6f306a8-59f7-41ae-b100-205804275dc1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/41c4b46d-9c2a-4028-847e-8ab9984e36d7" />
