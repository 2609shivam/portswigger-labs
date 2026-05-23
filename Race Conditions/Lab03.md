#  Single-endpoint Race Conditions

## 📌 Lab Details
- **Title**: Single-endpoint Race Conditions
- **Difficulty**: Practitioner
- **Category**: Race Conditions
- **Lab URL**: https://portswigger.net/academy/labs/launch/71c6fcf0f90655384236e0540795d7a265f823ea1258f995b615cf351fe32a56?referrer=%2fweb-security%2frace-conditions%2flab-race-conditions-single-endpoint

## 🔍 Summary
The application's email change functionality is vulnerable to a **single-endpoint race condition**. 
When multiple email change requests are sent simultaneously, the pending email value in the database can be overwritten before the confirmation email is fully processed.
This causes confirmation emails to be sent with mismatched data, allowing an attacker to receive a valid confirmation link for another user’s email address.

By exploiting this race condition, it is possible to claim: `carlos@ginandjuice.shop`

## 🛠 Steps to Solve
### Predict a potential collision
1. Log in and attempt to change your email to `anything@exploit-<YOUR-EXPLOIT-SERVER-ID>.exploit-server.net`. Observe that a confirmation email is sent to your intended new address, and you're prompted to click a link containing a unique token to confirm the change.
2. Complete the process and confirm that your email address has been updated on your account page.
3. Try submitting two different `@exploit-<YOUR-EXPLOIT-SERVER-ID>.exploit-server.net` email addresses in succession, then go to the email client.
4. Notice that if you try to use the first confirmation link you received, this is no longer valid. From this, you can infer that the website only stores one pending email address at a time. As submitting a new email address edits this entry in the database rather than appending to it, there is potential for a collision.

### Benchmark the behaviour
1. Send the `POST /my-account/change-email` request to Repeater.
2. In Repeater, add the new tab to a group.
3. Create 19 duplicate tabs. The new tabs are automatically added to the group.
4. In each tab, modify the first part of the email address so that it is unique to each request, for example, `test1@exploit-<YOUR-EXPLOIT-SERVER-ID>.exploit-server.net, test2@..., test3@...` and so on.
5. Send the group of requests in sequence over separate connections.
6. Go back to the email client and observe that you have received a single confirmation email for each of the email change requests.

### Probe for clues
1. In Repeater, send the group of requests again, but this time in parallel, effectively attempting to change the pending email address to multiple different values at the same time.
2. Go to the email client and study the new set of confirmation emails you've received. Notice that, this time, the recipient address doesn't always match the pending new email address.
3. Consider that there may be a race window between when the website:
   - Kicks off a task that eventually sends an email to the provided address.
   - Retrieves data from the database and uses this to render the email template.
4. Deduce that when a parallel request changes the pending email address stored in the database during this window, this results in confirmation emails being sent to the wrong address.

### Prove the concept
1. In Repeater, create a new group containing two copies of the `POST /my-account/change-email` request.
2. Change the `email` parameter of one request to `anything@exploit-<YOUR-EXPLOIT-SERVER-ID>.exploit-server.net`.
3. Change the `email` parameter of the other request to `carlos@ginandjuice.shop`.
4. Send the requests in parallel.
5. Check your inbox:
   - If you received a confirmation email in which the address in the body matches your own address, resend the requests in parallel and try again.
   - If you received a confirmation email in which the address in the body is `carlos@ginandjuice.shop`, click the confirmation link to update your address accordingly.
6. Go to your account page and notice that you now see a link for accessing the admin panel.
7. Visit the admin panel and delete the user `carlos` to solve the lab.

## 📖 Key Takeaways
- Race conditions are not limited to financial logic.
- Shared mutuable state is extremely dangerous.
- Asynchronous workflows often introduce exploitable timing windows.
- Parallel request testing is essential during security assessments.
- Burp Repeater tab groups are highly effective for race-condition discovery.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fbf80070-02c2-4bdd-8ae9-64096033550c" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/15e77b98-7c02-499d-a541-43634f379c1a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/84e2db33-d220-404e-b775-791f09c2c138" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/67cf4b98-144c-48f5-bd7e-dc565ae4688b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/1e9fb9f7-8136-40e3-b5c1-dfbfdb6bb8ad" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2d0ceddd-c76e-4fb5-9da8-144bcb7fe576" />
