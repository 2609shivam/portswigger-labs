# XSS - DOM XSS in document.write sink using source location.search inside a select element

## 📌 Lab Details
- **Title**: DOM XSS in document.write sink using source location.search inside a select element
- **Difficulty**: Apprentice 
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/20f0960254c2299d1e38d95c6131eed58e897552311335c61ea0c8d324d7ca71?referrer=%2fweb-security%2fcross-site-scripting%2fdom-based%2flab-document-write-sink-inside-select-element

## 🔍 Summary
This lab demonstrates a `DOM-based XSS` vulnerability where user input from `location.search` is injected into the page using `document.write()` inside a `<select>` element. By injecting a payload that breaks out of the `<select>` tag, an attacker can execute JavaScript.

## 🛠 Steps to Solve
1. Visit a product page.
2. Modify the URL to include a harmless `storeID` (e.g., `&storeId=abcd1234`) and verify it appears in the dropdown.
3. Confirm that this value is inside a `<select>` via `document.write()`.
4. Modify the URL to break out of the `<select>` and inject a script: `?productId=1&storeId="></select><img%20src=1%20onerror=alert(1)>`.
5. Load the page and observe the alert.
   
## 📖 Key Takeaways
- `document.write()` with unsanitized input is dangerous.
- Injecting user input inside HTML contexts like `<select>` makes it easier to break structure and inject malicious code.
- DOM-based XSS can occur even without server-side vulnerabilities—just pure JavaScript misuse.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/338a6036-3679-49c3-923d-6752d42ff90d)
![image](https://github.com/user-attachments/assets/a3547e3a-0dc9-4d7d-af9b-405262fda828)
![image](https://github.com/user-attachments/assets/caad43a4-d4d8-4456-8763-b0a9d5efc78f)
![image](https://github.com/user-attachments/assets/6afa6ef7-3584-4cd3-a436-f3743b22c6bc)
![image](https://github.com/user-attachments/assets/33439689-c76e-44cd-bfc6-65f319ed6bd8)
![image](https://github.com/user-attachments/assets/e38f5ea5-1277-4064-9711-1302e7716b5e)
