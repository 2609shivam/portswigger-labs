#  Insufficient Workflow Validation

## 📌 Lab Details
- **Title**: Insufficient Workflow Validation
- **Difficulty**: Practitioner
- **Category**: Business Logic
- **Lab URL**: https://portswigger.net/academy/labs/launch/b20989c23ae664ef4d1f4d060455e20d6c019ae17819049dfad91790a7c55765?referrer=%2fweb-security%2flogic-flaws%2fexamples%2flab-logic-flaws-insufficient-workflow-validation

## 🔍 Summary
The application does not properly validate the checkout workflow. By directly accessing the order confirmation endpoint, it’s possible to complete a purchase **without paying**.

## 🛠 Steps to Solve
1. Login using `wiener:peter`.
2. Buy a cheap item and observe requests in Burp.
3. Identify:
   ```sh
   GET /cart/order-confirmation?order-confirmation=true
   ```
4. Send this request to Repeater.
5. Add the **leather jacket** to cart.
6. Resend the request -> order completes without payment.

## 📖 Key Takeaways
- Business logic flaws arise from incorrect workflow assumptions.
- Critical actions must be validated server-side.
- Never allow direct access to sensitive endpoints without checks.

## 🖼️ Screenshot 
<img width="1919" height="647" alt="image" src="https://github.com/user-attachments/assets/92fcb322-7dec-4a67-adaf-15da22c90c09" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6c9f30d4-d78f-40f5-8410-eb251ce1fc0b" />
