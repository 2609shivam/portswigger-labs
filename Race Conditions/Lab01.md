#  Bypassing Rate Limits via Race Conditions

## 📌 Lab Details
- **Title**: Bypassing Rate Limits via Race Conditions
- **Difficulty**: Practitioner
- **Category**: Race Conditions
- **Lab URL**: https://portswigger.net/academy/labs/launch/29118bf96eda68696455206d01f5dcd44e5dfd57fc51f8be70d4146bf8c49072?referrer=%2fweb-security%2frace-conditions%2flab-race-conditions-bypassing-rate-limits

## 🔍 Summary
The application implements rate limiting on the login endpoint to prevent brute-force attacks. However, the rate-limit counter update is vulnerable to a **race condition**.
By sending multiple login attempts simultaneously, it is possible to exceed the allowed number of failed attempts before the lockout mechanism activates.

## 🛠 Steps to Solve
### Predict a potential collision
1. Experiment with the login function by intentionally submitting incorrect passwords for your own account.
2. Observe that if you enter the incorrect password more than three times, you're temporarily blocked from making any more login attempts for the same account.
3. Try logging in using another arbitrary username and observe that you see the normal Invalid username or password message. This indicates that the rate limit is enforced per-username rather than per-session.
4. Deduce that the number of failed attempts per username must be stored server-side.
5. Consider that there may be a race window between:
   - When you submit the login attempt
   - When the website increments the counter for the number of failed login attempts associated with a particular username.

### Benchmark the behavior
1. From the proxy history, find a `POST /login` request containing an unsuccessful login attempt for your own account.
2. Send this request to Burp Repeater.
3. In Repeater, add the new tab to a group.
4. Right-click the grouped tab, then select **Duplicate tab**. Create 19 duplicate tabs. The new tabs are automatically added to the group.
5. Send the group of requests in sequence, using separate connections to reduce the chance of interference.
6. Observe that after two more failed login attempts, you're temporarily locked out as expected.

### Probe for clues
1. Send the group of requests again, but this time in parallel.
2. Study the responses. Notice that although you have triggered the account lock, more than three requests received the normal `Invalid username and password` response.
3. Infer that if you're quick enough, you're able to submit more than three login attempts before the account lock is triggered.

### Prove the concept
1. Still in Repeater, highlight the value of the `password` parameter in the `POST /login` request.
2. Send the request to **Turbo Intruder**.
3. In Turbo Intruder, in the request editor, notice that the value of the `password` parameter is automatically marked as a payload position with the `%s` placeholder.
4. Change the `username` parameter to `carlos`.
5. Use the following script:
   ```sh
   def queueRequest(target, wordlists):
       engine=RequestEngine(endpoint=target.endpoint, concurrentConnections=1, engine=Engine.BURP2)
       passwords=wordlists.clipboard

       for password in passwords:
           engine.queue(target.req, password, gate='1')
       engine.openGate('1')

   def handleResponse(req, interesting):
       table.add(req)
   ```
6. Note that we're assigning the password list from the clipboard by referencing `wordlists.clipboard`. Copy the list of candidate passwords to your clipboard.
7. Launch the attack.
8. If you get a 302 response, that will contain the password.
9. Access the admin panel and delete the user `carlos` to solve the lab.

## 📖 Key Takeaways
- Rate limiting can fail under concurrency.
- Race conditions often appear in authentication logic.
- Single-packet attacks maximize simultaneity.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/aca974a1-9ccd-4ed1-9eea-be84f1cf6a1a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/410f74d8-e9e7-4bf3-9d93-5d6ca51b9b2b" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8f12304d-139f-4bd3-972c-b9ec259f7dd1" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/09b502ac-aad7-464a-a4a2-c1016106316e" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f4963f09-d49e-4682-ad52-ae0a8b04faf7" />
