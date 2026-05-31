#  Information Disclosure in Version Control History

## 📌 Lab Details
- **Title**: Information Disclosure in Version Control History
- **Difficulty**: Practitioner
- **Category**: Information Disclosure
- **Lab URL**: https://portswigger.net/academy/labs/launch/45ef28e2b8b6f9b623c8d0485cb4e2b7ded96458fedd8d7d5e0adf9427a5d3af?referrer=%2fweb-security%2finformation-disclosure%2fexploiting%2flab-infoleak-in-version-control-history

## 🔍 Summary
The application exposes its `.git` directory, allowing attackers to download the Git repository and inspect its commit history. Although sensitive information was removed from the latest version or the code, it remained accessible through previous commits.
By reviewing historical commits, an administrator password was recovered and used to gain administrative access to the application.

## 🛠 Steps to Solve
1. Open the lab and browse to `/.git` to reveal the lab's Git version control data.
2. Download a copy of this entire directory.
   ```sh
   wget -r https://YOUR-LAB-ID.web-security-academy.net/.git/
   ```
3. Explore the downloaded directory using your local Git installation. Notice that there is a commit with the message `"Remove admin password from config"`.
4. Look closer at the diff for the changed `admin.conf` file. Notice that the commit replaced the hard-coded admin password with an environment variable `ADMIN_PASSWORD` instead. However, the hard-coded password is still clearly visible in the diff.
5. Log in as the administrator and delete the user carlos to solve the lab.

## 📖 Key Takeaways
- Sensitive information should never be committed to version control.
- Deleting secrets in later commits does not remove them from Git history.
- Exposed `.git` directories can allow attackers to reconstruct source code and retrieve historical secrets.
- Git commit history is a common source of credential leakage during security assessments.

## 🖼️ Screenshot 
<img width="1918" height="822" alt="image" src="https://github.com/user-attachments/assets/285ed620-15ac-4310-925c-1f705957bca0" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d590098b-977b-45d2-ba56-f7b1db7fc845" />
