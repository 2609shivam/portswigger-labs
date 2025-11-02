#  Inconsistent handling of exceptional input

## 📌 Lab Details
- **Title**: Inconsistent handling of exceptional input
- **Difficulty**: Practitioner
- **Category**: Business Logic
- **Lab URL**: https://portswigger.net/academy/labs/launch/2cfddd3fae848e161277e28cc5cc7782f2b4baa2cbefbd2a3dce178fe656ad17?referrer=%2fweb-security%2flogic-flaws%2fexamples%2flab-logic-flaws-inconsistent-handling-of-exceptional-input

## 🔍 Summary
The app’s registration flow mishandles exceptionally long email input and string truncation. By registering with a purposely long address that includes `dontwannacry.com` as a subdomain and causing the server to truncate the stored address at 255 characters, an attacker can make the application think the account belongs to `@dontwannacry.com` while the email is actually delivered to the attacker’s mailbox.

## 🛠 Steps to Solve


## 📖 Key Takeaways
- **Never trust client-side** or **single-layer input checks**. Validation must be done server-side and consistently
- **Length checks are security controls**. Enforce maximum lengths consistently, and reject inputs that would be truncated rather than silently truncating.
- **Log and monitor suspicious registrations**. Long or unusual email addresses, especially those that nearly hit length limits, are indicators of abuse.

## 🖼️ Screenshot 
