#  Lab: Client-side desync

## 📌 Lab Details
- **Title**: Lab: Client-side desync
- **Difficulty**: Expert
- **Category**: HTTP Request Smuggling
- **Lab URL**: https://portswigger.net/academy/labs/launch/c7e1e3ecc45431f9694ade69d2c269fdfedcff87aa2cb4525dd503090d7d467e?referrer=%2fweb-security%2frequest-smuggling%2fbrowser%2fclient-side-desync%2flab-client-side-desync

## 🔍 Summary
Client-side desync is a request-smuggling variant where a server (or proxy) ignores `Content-Length`, letting attacker-supplied bytes inside a browser POST be parsed as a second HTTP request. In the lab you confirm the desync in Burp, reproduce it from a real browser with `fetch()`, use the comment feature as a gadget to store leaked request text, exfiltrate a victim’s `session` cookie, and reuse that cookie to access the victim account.

## 🛠 Steps to Solve


## 📖 Key Takeaways
- **Desync vector**: If front-end and back-end disagree about message boundaries (e.g., server ignores `Content-Length`), attacker-controlled payloads inside a POST body can be parsed as a new request.
- **Client-side trigger**: Modern browsers can be induced to send those bytes using `fetch()` (or similar) so the attack is executed from the victim’s browser — no direct server-side exploit needed.
- **Tight tuning**: The success depends on carefully chosen `Content-Length` values (capture size must be > your nested body prefix but < follow-up request size). Iteration is normal.
- **Impact**: Stolen session cookies let the attacker hijack the victim’s account without needing traditional XSS or CSRF.

## 🖼️ Screenshot 
