#  Developing a Custom Gadget Chain for Java Deserialization

## 📌 Lab Details
- **Title**: Developing a Custom Gadget Chain for Java Deserialization
- **Difficulty**: Expert
- **Category**: Insecure Deserialization
- **Lab URL**: https://portswigger.net/academy/labs/launch/bc6c96fd9ecd7550f07fba8ec2535c8612837e50a4c1a64aaf05f06c848a4f00?referrer=%2fweb-security%2fdeserialization%2fexploiting%2flab-deserialization-developing-a-custom-gadget-chain-for-java-deserialization

## 🔍 Summary
The application stores a serialized Java object inside the session cookie and deserializes it on the server without proper validation. <br>
During deserialization, the readObject() method inside the ProductTemplate class executes automatically. This method uses attacker-controlled input directly in a SQL query, leading to SQL injection through Java deserialization.

## 🛠 Steps to Solve
### Identifying the vulnerability
1. Log in to your own account and notice the session cookie contains a serialized Java object.
2. From the site map, notice that the website references the file `/backup/AccessTokenUser.java`.
3. Navigate upward to the `/backup` directory and notice that it also contains a `ProductTemplate.java` file.
4. Notice that the `ProductTemplate.readObject()` method passes the template's `id` attribute into a SQL statement.

### Template
Here is a ready to use program that you can run to test the payloads: https://github.com/PortSwigger/serialization-examples/tree/master/java/solution

### Extract the password
Here I will make changes in the Java file to test different payloads.
1. Enumerate the number of columns in the table: `' ORDER BY x--`. Trying this payload till an error is generated, `x-1` will give the number of columns.
2. Determine the data type of columns, by passing an arbitary string in each column, an error indicates that the column does not accept string.
3. List the contents of the database: `SELECT * FROM information_schema.tables`.
4. Use a suitable SQL Injection payload to extract the password: `' UNION SELECT NULL, NULL, NULL, CAST(password AS numeric), NULL, NULL, NULL, NULL FROM users--`
5. To solve the lab, log in as administrator using the extracted password, open the admin panel, and delete carlos.

## 📖 Key Takeaways
- Magic methods like `readObject()` can automatically execute attacker-controlled logic.
- Source code disclosure can massively simplify exploitation.
- SQL injection can occur indirectly through deserialization.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d5ad2069-9212-45d7-9316-43c83889f2e3" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/63bcd67c-c131-4636-ba0b-b66100a7e79b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/99ded28e-23eb-4410-85b0-80df3ab23c4f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/446c866c-814d-43a6-8cc8-8f3691fc107a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/45eff734-edc9-4b0f-98b5-bf73d7b4058e" />
