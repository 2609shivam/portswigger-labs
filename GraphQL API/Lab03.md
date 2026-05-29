# Bypassing GraphQL Brute Force Protections

## 📌 Lab Details
- **Title**: Bypassing GraphQL Brute Force Protections
- **Difficulty**: Practitioner
- **Category**: GraphQL API
- **Lab URL**: https://portswigger.net/academy/labs/launch/1ff12f295e639070b0200de527e0d58cd54ffe26907088691be7155d78305dc9?referrer=%2fweb-security%2fgraphql%2flab-graphql-brute-force-protection-bypass

## 🔍 Summary
The application's login functionality uses a GraphQL API protected by rate limiting.
Normally, repeated login attempts trigger the rate limiter, preventing traditional brute-force attacks.
However, GraphQL aliases allow multiple mutations to be bundled into a single request. By abusing aliases, an attacker can perform many login attempts simultaneously while only sending one HTTP request, effectively bypassing the rate limiter.
This enables brute-force attacks against user credentials despite server-side protections.

## 🛠 Steps to Solve
1. In the browser, access the lab and select **My account**.
2. Attempt to log in to the site using incorrect credentials.
3. Send the login request to **Burp Repeater**.
4. In Repeater, attempt some further login requests with incorrect credentials. Note that after a short period of time the API starts to return a rate limit error.
5. In the GraphQL tab, craft a request that uses aliases to send multiple login mutations in one message. Use the following script in the browser console:
   ```sh
   copy(`123456,password,12345678,qwerty`.split(',').map((element,index)=>`
   bruteforce$index:login(input:{password: "$password", username: "carlos"}) {
        token
        success
    }
   `.replaceAll('$index',index).replaceAll('$password',element)).join('\n'));
   ```
   The list of aliases should be contained within a mutation {} type.
6. Notice that the response lists each login attempt and whether its login attempt was successful.
7. Search for the string with the `true` response to find the credentials.
8. Log in to the site using the `carlos` credentials to solve the lab.

## 📖 Key Takeaways
- GraphQL aliases can execute multiple operations in one request.  
- Rate limiting at the HTTP layer alone is insufficient.
- Authentication protections must account for GraphQL query structure.
- Query complexity analysis is essential for GraphQL security.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/127ec3ea-1ff2-438a-ae16-b77d5a87e000" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/da1acaa1-39c3-4fae-9a3f-c437f6d0703a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/70c5b356-baef-49b4-b56c-9d79cb42ae52" />
