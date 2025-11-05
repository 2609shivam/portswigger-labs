#  Inconsistent handling of exceptional input

## 📌 Lab Details
- **Title**: Inconsistent handling of exceptional input
- **Difficulty**: Practitioner
- **Category**: Business Logic
- **Lab URL**: https://portswigger.net/academy/labs/launch/2cfddd3fae848e161277e28cc5cc7782f2b4baa2cbefbd2a3dce178fe656ad17?referrer=%2fweb-security%2flogic-flaws%2fexamples%2flab-logic-flaws-inconsistent-handling-of-exceptional-input

## 🔍 Summary
The app’s registration flow mishandles exceptionally long email input and string truncation. By registering with a purposely long address that includes `dontwannacry.com` as a subdomain and causing the server to truncate the stored address at 255 characters, an attacker can make the application think the account belongs to `@dontwannacry.com` while the email is actually delivered to the attacker’s mailbox.

## 🛠 Steps to Solve
1. While proxying traffic through Burp, open the lab and go to the "Target" > "Site map" tab. Right-click on the lab domain and select "Engagement tools" > "Discover content" to open the content discovery tool.
2. Click "Session is not running" to start the content discovery. After a short while, look at the "Site map" tab in the dialog. Notice that it discovered the path `/admin`.
3. Try to browse to `/admin`. Although you don't have access, an error message indicates that `DontWannaCry` users do.
4. Go to the account registration page. Notice the message telling `DontWannaCry` employees to use their company email address.
5. From the button in the lab banner, open the email client. Make a note of the unique ID in the domain name for your email server (`@YOUR-EMAIL-ID.web-security-academy.net`).
6. Go back to the lab and register with an exceptionally long email address in the format:
   ```sh
   very-long-string@YOUR-EMAIL-ID.web-security-academy.net
   ```
   The `very-long-string` should be at least 200 characters long.
7. Go to the email client and notice that you have received a confirmation email. Click the link to complete the registration process.
8. Log in and go to the "My account" page. Notice that your email address has been truncated to 255 characters.
9. Log out and go back to the account registration page.
10. Register a new account with another long email address, but this time include `dontwannacry.com` as a subdomain in your email address as follows:
    ```sh
    very-long-string@dontwannacry.com.YOUR-EMAIL-ID.web-security-academy.net
    ```
    Make sure that the very-long-string is the right number of characters so that the "m" at the end of `@dontwannacry.com` is character 255 exactly.
11. Go to the email client and click the link in the confirmation email that you have received. Log in to your new account and notice that you now have access to the admin panel.
12. Go to the admin panel and delete `carlos` to solve the lab.

## 📖 Key Takeaways
- **Never trust client-side** or **single-layer input checks**. Validation must be done server-side and consistently
- **Length checks are security controls**. Enforce maximum lengths consistently, and reject inputs that would be truncated rather than silently truncating.
- **Log and monitor suspicious registrations**. Long or unusual email addresses, especially those that nearly hit length limits, are indicators of abuse.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d9585d7c-9470-44c5-b854-4dc0e649674f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7269114d-f69f-448c-8782-c97bfec2d79f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/73134050-a8f3-46d2-b3ce-3027389a45bc" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d880dd3e-3588-4185-a8a3-898b21129f2e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a6018659-0493-4638-ae0d-beb32720ff02" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4d80377e-ad2e-4b4d-afd6-87f2cf8a33fc" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/09cb1859-5b04-4b25-84f1-28688c772688" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/60b3a528-b6b1-4b23-b4ef-e4fc93e22744" />
