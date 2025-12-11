#  Infinte money logic flaw

## 📌 Lab Details
- **Title**: Infinte money logic flaw
- **Difficulty**: Practitioner
- **Category**: Business Logic
- **Lab URL**: https://portswigger.net/academy/labs/launch/4481d16f7bd443604bf74cef06da6df4b6da2b010f6f5a6b33358551d560c9d0?referrer=%2fweb-security%2flogic-flaws%2fexamples%2flab-logic-flaws-infinite-money

## 🔍 Summary
Automated a buy-extract-redeem loop to turn discounted $10 gift-card purchases into repeated store-credit by exploiting a logic flaw.

## 🛠 Steps to Solve
1. With Burp running, log in and sign up for the newsletter to obtain a coupon code, `SIGNUP30`. Notice that you can buy $10 gift cards and redeem them from the **My account** page.
2. Add a gift card to your basket and proceed to the checkout. Apply the coupon code to get a 30% discount. Complete the order and copy the gift card code to your clipboard.
3. Go to your account page and redeem the gift card. Observe that this entire process has added $3 to your store credit. Now we need to try and automate this process.
4. Study the proxy history and notice that ti redeem the gift card by supplying the code in the gift-card parameter of the `POST /gift-card` request.
5. Open the **Settings** dialog. Click **Sessions**. In the **Session handling rules panel**, click Add. The Session handling rule editor dialog opens.
6. In the dialog, go to the **Scope** tab. Under **URL scope**, select **Include all URLs**.
7. In the **Details** tab, under **Rule actions**, click Add > Run a macro. Under Select macro, click Add again to open the Macro Recorder.
8. Select the following sequence of requests:
   ```sh
   POST /cart
   POST /cart/coupon
   POST /cart/checkout
   GET /cart/order-confirmation?order-confirmed=true
   POST /gift-card
   ```
9. In the list of requests, select `GET /cart/order-confirmation?order-confirmed=true`. Click **Configure item**. In the dialog that opens, click Add to create a custom parameter. Name the parameter **gift-card** and highlight the gift card code at the bottom of the response. Click OK twice to go back to the Macro Editor.
10. Select the `POST /gift-card` request and click Configure item again. In the Parameter handling section, use the drop-down menus to specify that the `gift-card` parameter should be derived from the prior response (response 4). Click OK.
11. In the Macro Editor, click Test macro. Look at the response to `GET /cart/order-confirmation?order-confirmation=true` and note the gift card code that was generated. Look at the `POST /gift-card request`. Make sure that the gift-card parameter matches and confirm that it received a `302` response. Keep clicking OK until you get back to the main Burp window.
12. Send the `GET /my-account` request to Burp Intruder. Make sure that **Sniper** attack is selected.
13. Select the payload type as **Null Payloads**. Choose to generate 412 payloads. Set the **Maximum concurrent requests** to 1. Start the attack.
14. When the attack finishes, you will have enough store credit to buy the jacket and solve the lab.

## 📖 Key Takeaways
- **Macro + session rule = automation**: record the full sequence (add→coupon→checkout→order confirmation→redeem) and have Burp run it automatically.
- **Dynamic extraction & injection**: grab the generated gift-card code from the order-confirmation response and inject it into the redeem request in the same macro loop.
- **Use of a benign Intruder target**: point Intruder at a harmless endpoint (here GET /my-account) so each iteration triggers the macro without changing meaningful request data.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d61b51ec-56f5-457e-87b7-c7ffa8e3cedc" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5b5ba2bd-ddc5-4d60-8cb1-b467414c65de" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c1cb6669-47bc-49c6-966a-7f1324d76d51" />
<img width="1444" height="895" alt="image" src="https://github.com/user-attachments/assets/b9649433-d788-4dbb-bca1-778e6f10a166" />
<img width="920" height="1072" alt="image" src="https://github.com/user-attachments/assets/89db5c99-e8a2-495f-90ee-495a348e4f0e" />
<img width="1232" height="1009" alt="image" src="https://github.com/user-attachments/assets/50172711-4493-49a2-87aa-65e7c5d5912b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e52a7e20-d1e2-4b31-b932-7f475392f595" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8edea5cc-0060-4509-a8f0-791afa600e69" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fe61fb92-51ba-4938-bfd9-252d137586af" />
