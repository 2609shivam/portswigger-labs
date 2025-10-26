#  2FA Simple Bypass

## 📌 Lab Details
- **Title**: 2FA Simple Bypass 
- **Difficulty**: Apprentice
- **Category**: Authentication
- **Lab URL**: https://portswigger.net/academy/labs/launch/55a6f6d2de02362486d2c507f3191adb99fb573cd1583e7a69246d62445ed703?referrer=%2fweb-security%2fauthentication%2fmulti-factor%2flab-2fa-simple-bypass

## 🔍 Summary
The lab shows a logic flaw where completing 2FA is enforced only on the client/flow level — an attacker who logs in as another user can manually navigate to a protected page (/my-account) and access it without completing the victim’s 2FA.

## 🛠 Steps to Solve
1. Log in to your own account. Your 2FA verification code will be sent to you by email. Click the **Email client** button to access your emails.
2. Go to your account page and make a note of the URL.
3. Log out of your account.
4. Log in using the victim's credentials.
5. When prompted for the verification code, manually change the URL to navigate to `/my-account`. The lab is solved when the page loads.

## 📖 Key Takeaways
- 2FA enforcement must be server-side (don’t rely on URL flow or client checks).
- Protect sensitive endpoints with an authoritative server-side flag that requires successful 2FA before returning account data.
- Tie session/auth state to server-side tokens (mark 2fa_completed = true only after verification).

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bfc27ae1-d316-4a38-980d-191b48301d3a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/58cf26a6-d9a0-4254-964a-21c3aa68eaed" />
