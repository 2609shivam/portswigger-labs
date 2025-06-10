# XSS - Reflected XSS with event handlers

## 📌 Lab Details
- **Title**: Reflected XSS with event handlers and href attributes blocked
- **Difficulty**: Expert
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/8d3b6a32167c58218b1807a6070d8d24bd5fd88ac74f6eb74127cd73aae46cb8?referrer=%2fweb-security%2fcross-site-scripting%2fcontexts%2flab-event-handlers-and-href-attributes-blocked

## 🔍 Summary
This lab contains a **reflected XSS** vulnerability with event handlers and `href` attributes blocked. A dynamic attribute `<animate>` is used to dynamically set the value of `href` and trigger an alert.

## 🛠 Payload Used
```sh
https://0ab2006f0455bb4e805d263700380088.web-security-academy.net/?search=%3Csvg%3E%3Ca%3E%3Canimate+attributeName%3Dhref+values%3Djavascript%3Aalert(1)+%2F%3E%3Ctext+x%3D20+y%3D20%3EClick%20me%3C%2Ftext%3E%3C%2Fa%3E
```

## 📖 Key Takeaways
- You **bypass** `href` and `onclick` **filtering** using **SVG animation** to set a `javascript:` URL dynamically.
- The `animate` tag changes the attribute **after the page loads**, avoiding static filter.
- The attack runs when the user clicks the animated link labeled **Click me**.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/5fd4be4d-a504-4df7-a4cc-672736444178)
![image](https://github.com/user-attachments/assets/a5bd0422-8b03-4d64-95bd-2b6f6f5d915e)
![image](https://github.com/user-attachments/assets/432d5c58-08e5-4c9d-8207-d74e72892657)
