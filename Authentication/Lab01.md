#  Broken brute-force protection, IP block

## 📌 Lab Details
- **Title**: Broken brute-force protection, IP block
- **Difficulty**: Practitioner
- **Category**: Authentication
- **Lab URL**: https://portswigger.net/academy/labs/launch/6d4cc1c44751ee06710496ea067aa78c8b2cb01e7686737e13a7e88261d1471a?referrer=%2fweb-security%2fauthentication%2fpassword-based%2flab-broken-bruteforce-protection-ip-block

## 🔍 Summary
The lab demonstrates a logic flaw in brute-force protection: the application blocks an IP after three failed logins, but that counter can be reset by logging in as a different (attacker-controlled) user. By alternating a valid reset login (wiener:peter) with brute-force attempts for the victim (carlos), an attacker can bypass the IP block and eventually find the victim’s password (then log in and view the account).

## 🛠 Steps to Solve
1. Investigate the login page and observe that your **IP** is temporarily blocked if you submit 3 incorrect logins in a row. However, notice that you can reset the counter for the number of failed login attempts by logging in to your own account before this limit is reached.
2. Intercept the `POST /login` request to Burp Intruder. Create a pitchfork attack with payload positions in both the `username` and `password` parameters.
3. Set the **Maximum concurrent Requests** to 1 in the **Resource pool** side panel.
4. In the **Payloads** side panel, for the username, add a list of payloads that alternate between your username and **carlos**. For the **passwords**, edit the list of candidate passwords and add your own password before each one.
5. When the attack finishes, filter the results to hide responses with a **200** status code. Sort the remaining results by username. There should only be a single **302** response for requests with the username **carlos**.

## 📖 Key Takeaways
- **Root cause** — flawed state reset: The failed-attempt counter is reset (or not tracked per-account properly) by actions that an attacker can trigger, letting them avoid the block.
- **Detection hints**: Unusual pattern of interleaved successful logins from one account and repeated failed attempts for another, or many short bursts of login activity separated by successful logins
- **Operational defenses**: log and alert on high-frequency alternating login patterns, and require multi-factor authentication (MFA) for sensitive accounts.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a1baa75c-2df4-4edb-b71e-fce2c086a9c8" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/94364013-3f5c-4b6c-bcd5-0114a2d8a5a7" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9764ac46-7d4f-4ca4-9c95-5a941ce42343" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/aae9a456-a6cf-41be-99e9-bc6d6935f194" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f5742b40-985b-4138-a3d0-35c899ba188b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4d898c5e-fb0f-4147-b303-2faf8b67a563" />
