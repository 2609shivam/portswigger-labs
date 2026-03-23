#  Bypassing access controls

## 📌 Lab Details
- **Title**: Bypassing access controls using email address discrepancies
- **Difficulty**: Expert
- **Category**: Business Logic
- **Lab URL**: https://portswigger.net/academy/labs/launch/07b1444427ce26a6ad6a4eafdd9a2500cabaef214aa99c6332f8d7f4bdf1e885?referrer=%2fweb-security%2flogic-flaws%2fexamples%2flab-logic-flaws-bypassing-access-controls-using-email-address-parsing-discrepancies

## 🔍 Summary
The application restricts registration to emails from: `ginandjuice.shop`.
However, due to a **parser discrepancy**, the validation logic and email processing interpret encoded emails differently. By using **UTF-7 encoding**, we can bypass validation and recieve the verification email on our exploit server.

## 🛠 Steps to Solve
1. Confirm restriction: `foo@exploit-server.net -> blocked`.
2. Test encodings:
   - UTF-8 / ISO -> blocked
   - UTF-7 -> allowed
3. Exploit payload:
   ```sh
   =?utf-7?q?attacker&AEA-[YOUR-ID]&ACA-?=@ginandjuice.shop
   ```
4. Register account by submitting payload as email.
5. Check **Exploit** server for verification email.
6. Login to the admin panel and delete the user carlos.

## 📖 Key Takeaways
- Parser mismatch = **validation bypass**.
- UTF-7 encoding can evade filters.
- Always compare **properly parsed domains**, not raw input.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1c86cc1a-18c7-434c-bb40-73574e1ea6af" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/de9ed5d8-d500-49d2-bef4-1d9870b805bc" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f880a2b7-ce0f-445b-b000-3edcb6a3eba5" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2a61ab54-fca2-45c5-8009-9f17c46018c5" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e3e5e68e-a26a-48bd-9863-db92bb2981e5" />
