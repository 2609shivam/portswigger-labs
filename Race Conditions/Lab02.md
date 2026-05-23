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
### Predict a potential collision
1. Log in and purchase a gift card so you can study the purchasing flow.
2. Consider that the shopping cart mechanism and, in particular, the restrictions that determine what you are allowed to order, are worth trying to bypass.
3. From the proxy history, identify all endpoints that enable you to interact with the cart. For example, a `POST /cart` request adds items to the cart and a `POST /cart/checkout` request submits your order.
4. Add another gift card to your cart, then send the `GET /cart` request to Burp Repeater.
5. In Repeater, try sending the `GET /cart` request both with and without your session cookie. Confirm that without the session cookie, you can only access an empty cart. From this, you can infer that:
   - The state of the cart is stored server-side in your session.
   - Any operations on the cart are keyed on your session ID or the associated user ID.
6. Notice that submitting and receiving confirmation of a successful order takes place over a single request/response cycle.
7. Consider that there may be a race window between when your order is validated and when it is confirmed. This could enable you to add more items to the order after the server checks whether you have enough store credit.

### Benchmark the behaviour
1. Send both the `POST /cart` and `POST /cart/checkout` request to Burp Repeater.
2. In Repeater, add the two tabs to a new group.
3. Send the two requests in sequence over a single connection a few times. Notice from the response times that the first request consistently takes significantly longer than the second one.
4. Add a `GET` reqeust for the homepage to the start of your tab group.
5. Send all three requests in sequence over a single connection. Observe that the first request still takes longer, but by "warming" the connection in this way, the second and third requests are now completed within a much smaller window.
6. Deduce that this delay is caused by the back-end network architecture rather than the respective processing time of the each endpoint. Therefore, it is not likely to interfere with your attack.
7. Remove the `GET` request for the homepage from your tab group.
8. Make sure you have a single gift card in your cart.
9. In Repeater, modify the `POST /cart` request in your tab group so that the `productId` parameter is set to `1`, that is, the ID of the **Lightweight L33t Leather Jacket**.
10. Send the requests in sequence again.
11. Observe that the order is rejected due to insufficient funds, as you would expect.

### Prove the concept
1. Remove the jacket from your cart and add another gift card.
2. In Repeater, try sending the requests again, but this time in parallel.
3. Look at the response to the POST /cart/checkout request:
   - If you received the same "insufficient funds" response, remove the jacket from your cart and repeat the attack. This may take several attempts.
   - If you received a 200 response, check whether you successfully purchased the leather jacket. If so, the lab is solved.

## 📖 Key Takeaways
- Race conditions can occur across multiple endpoints.
- Shared session-based state is a common attack target.
- Validation and state updates must be atomic.
- Parallel request testing in Burp Repeater is extremely effective for detecting race conditions.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2deb6f40-1b6a-4bd2-a953-1a92eec39d7e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c487235b-2689-4540-ac71-02c4442707fe" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0137a935-9a7d-424b-aa35-61bca14d6aca" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d0b127d9-fa75-4950-bcff-27e0dc25bb4a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/68743623-0564-4515-8a63-a4d17293155a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cddab309-9b0b-4a14-8ca8-5f2ad3c9569d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/58ed8ce9-81e8-4b31-abfe-260328d2ea06" />
