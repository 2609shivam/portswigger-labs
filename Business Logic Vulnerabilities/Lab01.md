#  Low-level logic flaw

## 📌 Lab Details
- **Title**: Low-level logic flaw
- **Difficulty**: Practitioner 
- **Category**: Business Logic
- **Lab URL**: https://portswigger.net/academy/labs/launch/7d3ebe9916b96a36642a8bd0179888fbcddef63a401c684a077d9877a0966aa4?referrer=%2fweb-security%2flogic-flaws%2fexamples%2flab-logic-flaws-low-level

## 🔍 Summary
Triggered a **32-bit integer overflow** in the cart by repeatedly adding large quantities of an item via Burp Intruder. The total wrapped to a large negative value, allowing you to adjust quantities so the cart total fell within your $100 credit and complete the purchase for effectively free.

## 🛠 Steps to Solve
1. With Burp running, log in and attempt to buy the leather jacket. The order is rejected because you don't have enough store credit. In the proxy history, study the order process. Send the POST /cart request to Burp Repeater.
2. In Burp Repeater, notice that you can only add a 2-digit quantity with each request. Send the request to Burp Intruder.
3. Go to `Intruder` and set the **quantity** parameter to `99`.
4. Select the payload type **Null payloads**. Under **Payload configuration**, select **Continue indefinitely**. Start the attack.
5. While the attack is running, go to your cart. Keep refreshing the page every so often and monitor the total price. Eventually, notice that the price suddenly switches to a large negative integer and starts counting up towards 0. The price has exceeded the maximum value permitted for an integer in the back-end programming language (2,147,483,647). As a result, the value has looped back around to the minimum possible value (-2,147,483,648).
6. Clear your cart. In the next few steps, we'll try to add enough units so that the price loops back around and settles between $0 and the $100 of your remaining store credit. This is not mathematically possible using only the leather jacket. Note that the price of the jacket is stored in cents (133700).
7. Create the same Intruder attack again, but this time under **Payload configuration**, choose to generate exactly `323` payloads.
8. Set the **Maximum concurrent requests** to 1. Start the attack.
9. When the Intruder attack finishes, go to the `POST /cart` request in Burp Repeater and send a single request for `47 `jackets. The total price of the order should now be `-$1221.96`.
10. Use Burp Repeater to add a suitable quantity of another item to your cart so that the total falls between $0 and $100.
11. Place the order to solve the lab.

## 📖 Key Takeaways
- **Server-side validation**: always recalculate totals server-side from trusted unit prices and quantities; never trust client-supplied totals.
- **Idempotency & limits**: enforce reasonable per-request quantity limits and global rate/resource limits.
- **Logging & alerts**: detect abnormal quantity changes or sudden huge totals and alert for manual review.
- **Test for overflow**: include integer-overflow and high-volume sequence tests in QA/pentesting.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/167e4923-8f84-4654-87fe-65d2fd2b388c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/11673d03-c7a3-4174-a052-4f91bbd5f114" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5d1751e2-8de5-4029-9e8c-ad635322d3e2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a4235334-98c8-40e5-952b-e992f3a9bf62" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bcdc7353-4e1f-47e6-8eb8-b3c42d0dc1d2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1034a049-89de-4d4c-9dde-0bfa8eec92b7" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c645c3bd-94ca-4fdf-843d-c6cf77f3e55a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e89baa13-6606-4108-8779-91a64629b646" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/eceeeeb1-fcbe-4f2c-8043-cb9877a0235d" />
