#  Host Validation Bypass via Connection State Attack

## 📌 Lab Details
- **Title**: Host Validation Bypass via Connection State Attack
- **Difficulty**: Practitioner
- **Category**: HTTP Host header attacks
- **Lab URL**: https://portswigger.net/academy/labs/launch/3c3f012cd1e1e6555dfc01b4bdddb6740a83ec699d85741398157fdb74985d78?referrer=%2fweb-security%2fhost-header%2fexploiting%2flab-host-header-host-validation-bypass-via-connection-state-attack

## 🔍 Summary
This lab demonstrates a routing-based SSRF vulnerability caused by incorrect assumptions about connection state. <br>
The front-end server validates the `Host` header only for the first request received on a TCP connection. Subsequent requests sent through the same connection inherit the trust established by the initial request. <br>
By first sending a legitimate request and then sending a second request with a malicious `Host` header over the same connection, it becomes possible to bypass host validation and access internal resources.

## 🛠 Steps to Solve
1. Send the `GET /` request to Burp Repeater.
2. Make the following adjustments:
   - Change the path to `/admin`
   - Change `Host` header to `192.168.0.1`
3. Send the request. Observe that you are simply redirected to the homepage.
4. Duplicate the tab, then add both tabs to a new group.
5. Select the first tab and make the following adjustments:
   - Change the path back to `/`.
   - Change the `Host` header back to `YOUR-LAB-ID.h1-web-security-academy.net`.
6. Send the group in sequence (single connection).
7. Change the `Connection` header to `keep-alive`.
8. Send the sequence and check the responses. Observe that the second request has successfully accessed the admin panel.
9. Study the response and observe that the admin panel contains an HTML form for deleting a given user. Make a note of the following details:
    - The action attribute (`/admin/delete`)
    - The name of the input (`username`)
    - The `csrf` token.
10. On the second tab in your group, send a `POST` request to `/admin/delete` with `Host`,`csrf` and `username` parameter.
11. Send the requests in sequence down a single connection to solve the lab.

## 📖 Key Takeaways
- Host validation must be performed per request, not per connection.
- TCP connection reuse can create unexpected trust boundaries.
- Routing-based SSRF can occur even when direct Host header attacks appear blocked.
- Connection state attacks are closely related to modern request smuggling techniques.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5564e353-4b9c-417e-90dc-3cbce5306380" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7142ff06-2a7a-4c5b-accf-4b6114fc1875" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d57968dc-1a13-4f0f-9a46-de36498445c1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4c9d53af-2050-4ce1-afa2-4e1b4ad6edda" />
