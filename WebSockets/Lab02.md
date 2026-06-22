# Manipulating the WebSocket handshake to exploit vulnerabilities

## 📌 Lab Details
- **Title**: Manipulating the WebSocket handshake to exploit vulnerabilities
- **Difficulty**: Practitioner
- **Category**: WebSockets
- **Lab URL**: https://portswigger.net/academy/labs/launch/f745f6c86fd28635cf5bdc4071bcbbd639d2ce9755d8ab435c059fb2c96e90f4?referrer=%2fweb-security%2fwebsockets%2flab-manipulating-handshake-to-exploit-vulnerabilities

## 🔍 Summary
This lab demonstrates how vulnerabilities can arise during the WebSocket handshake process. The application includes a live chat feature that communicates over WebSockets and implements an aggressive XSS filter that blocks obvious payloads. <br>

When an XSS attempt is detected, the server immediately terminates the WebSocket connection and bans the client's IP address. However, the application incorrectly trusts the `X-Forwarded-For` header when determining the client's IP address. By spoofing this header during the WebSocket handshake, an attacker can bypass the IP ban, reconnect, and deliver an obfuscated XSS payload that bypasses the flawed filter. <br>

Successfully exploiting this vulnerability triggers JavaScript execution in the support agent's browser.

## 🛠 Steps to Solve
1. Click "Live chat" and send a chat message.
2. In Burp Proxy, go to the WebSockets history tab, and observe that the chat message has been sent via a WebSocket message.
3. Send the message to Repeater.
4. Edit and resend the message containing a basic XSS payload, such as:
   ```sh
   <img src=x onerror='alert(1)'>
   ```
5. Observe that the attack has been blocked, and that your WebSocket connection has been terminated.
6. Click "Reconnect", and observe that the connection attempt fails because your IP address has been banned.
7. Add the following header to the handshake request to spoof your IP address:
   ```sh
   X-Forwarded-For: 1.1.1.1
   ```
8. Click "Connect" to successfully reconnect the WebSocket.
9. Send a WebSocket message containing an obfuscated XSS payload, such as:
    ```sh
    <img src=1 oNeRrOr=alert`1`>
    ```

## 📖 Key Takeaways
- WebSocket security begins during the HTTP handshake.
- Trusting client-controlled headers such as `X-Forwarded-For` for security decisions is dangerous.
- IP bans can often be bypassed if reverse proxy headers are not properly validated.
- HTML attribute names are case-insensitive, allowing payloads like `oNeRrOr` to evade naive filters.

## 🖼️ Screenshot 
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/0a5c752d-8824-4246-b3a5-dd365718a6d3" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/b83495c0-2a00-498c-85ea-da172b7f4140" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/11052658-d60e-4c02-bf95-711e156aa495" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/1a4af303-be2d-45e2-a690-f8956f085325" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/db0735fd-88c0-422c-b625-70fa199d67ac" />
