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


## 📖 Key Takeaways


## 🖼️ Screenshot 
