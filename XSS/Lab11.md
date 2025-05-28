# XSS - DOM XSS in Angular JS

## 📌 Lab Details
- **Title**: DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded
- **Difficulty**: Practitioner
- **Category**: Cross-site Scripting
- **Lab URL**: https://portswigger.net/academy/labs/launch/7dd0e73fe7cd3372360932722c0dfa1b3d73637170c9107ab1fb90c1ae2b4ae1?referrer=%2fweb-security%2fcross-site-scripting%2fdom-based%2flab-angularjs-expression

## 🔍 Summary
This lab demonstrates a **DOM-based XSS** vulnerability in an AngularJS application. User input from the search box is reflected inside an `ng-app` directive and evaluated as an AngularJS expression.

## 🛠 Steps to Solve
1. Enter a random alphanumeric string into the search box.
2. View the page source and observe that your random string is enclosed in an `ng-app` directive.
3. Insert the payload into the search box: `{{$on.constructor('alert(1)')()}}`.
4. Click search.
   
## 📖 Key Takeaways
- This type of attack doesn't need `<script>` or `<img error>` -  just exploiting Angular's features.
- Putting user input into dynamic frameworks(Angular,React,Vue) can be just as dangerous as traditional XSS.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/447e2391-c143-4e01-979b-f13533c947b6)
![image](https://github.com/user-attachments/assets/c5bbec8f-4f9a-4b25-99ee-3056adf8c4a8)
![image](https://github.com/user-attachments/assets/7a9ce701-43c6-41c8-9ab6-ffcb4fced134)
![image](https://github.com/user-attachments/assets/8d576c4f-12c2-4475-ba49-2bc3f3e253b7)
