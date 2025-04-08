# SQL Injection - with filter bypass via XML encoding

## 📌 Lab Details
- **Title**: SQLi with filter bypass via XML encoding
- **Difficulty**: Practitioner
- **Category**: SQL Injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/c8332afeeb7acf103db621a4d16b7249c26f7040478a07ff274d2f98687b02b6?referrer=%2fweb-security%2fsql-injection%2flab-sql-injection-with-filter-bypass-via-xml-encoding

## 🔍 Summary
This lab demonstrated how `Hackvertor` extension can be used to bypass the web application firewall.

## 🛠 Steps to Solve
1. Checking for the existence of a web application firewall by executing SQLi in the stock category `UNION SELECT NULL`.
2. Encoding the SQL query into the database using the `Hackvertor` extension.
3. Retrieving the username and password from the database by concatenating both the columns `UNION SELECT username || '~' || password FROM users`.
   
## 📖 Key Takeaways
- Understanding the use of `Hackvertor` extension in bypassing the web application firewall.
  
## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/7788cc8f-c2f3-461d-a37e-193ae1173085)
![image](https://github.com/user-attachments/assets/d18c61f3-5266-42f3-9057-22f0cc3e0a22)
![image](https://github.com/user-attachments/assets/3f390d85-e3fa-41db-afd1-6579ef82e094)
![image](https://github.com/user-attachments/assets/10ec8d9c-1061-472b-a750-ffb2d5721a46)
![image](https://github.com/user-attachments/assets/f964c393-5698-4afb-8bd0-e9f6780cb252)
