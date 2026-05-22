#  Multi-endpoint race conditions

## 📌 Lab Details
- **Title**: Multi-endpoint race conditions
- **Difficulty**: Pracititioner
- **Category**: Race Conditions
- **Lab URL**: https://portswigger.net/academy/labs/launch/167394bd4478dd05f683affd8d0bf579e9daf428cb0808509ba8f76b1a12c046?referrer=%2fweb-security%2frace-conditions%2flab-race-conditions-multi-endpoint

## 🔍 Summary
This lab contains a race condition in the purchasing workflow that allows items to be purchased for an unintended price.
The vulnerability exists because cart modification and checkout occur through separate endpoints, creating a timing window where the cart contents can change after validation but before order completion.

## 🛠 Steps to Solve


## 📖 Key Takeaways
- Race conditions can occur across multiple endpoints.
- Shared session-based state is a common attack target.
- Validation and state updates must be atomic.
- Parallel request testing in Burp Repeater is extremely effective for detecting race conditions.

## 🖼️ Screenshot 
