#  Accidental exposure of private GraphQL fields

## 📌 Lab Details
- **Title**: Accidental exposure of private GraphQL fields
- **Difficulty**: Practitioner
- **Category**: GraphQL API
- **Lab URL**: https://portswigger.net/academy/labs/launch/ccda6760929ebeccab51242f59b1573dd478087800b63c88fc8aa308db137802?referrer=%2fweb-security%2fgraphql%2flab-graphql-accidental-field-exposure

## 🔍 Summary
The application exposes sensitive GraphQL fields through introspection and improperly secured queries.
By using GraphQL introspection, it was possible to discover hidden queries and schema details. A query named `getUser` exposed both the `username` and `password` fields for arbitary users based solely on an `id` value.
Since the API lacked proper authorization checks, an attacker could enumerate user IDs and retrieve administrator credentials directly from the GraphQL endpoint.

## 🛠 Steps to Solve
1. Access the lab and select **My account**.
2. Send the GraphQL mututation request to **Burp Repeater**.
3. In Repeater, right-click anywhere within the Request panel of the message editor and select **GraphQL > Set introspection query** to insert an introspection query into the request body.
4. Send the request.
5. Right-click the message and select **GraphQL > Save GraphQL queries to site map**.
6. Go to **Target > Site map** and review the GraphQL queries. Notice the following:
   - There is a getUser query that returns a user's `username` and `password`.
   - This query fetches the relevant user information via a direct reference to an `id` number.
7. Send the `getUser` query to **Burp Repeater**.
8. Test alternative values of `id` to find the administrator's credentials.
9. Log in as administrator and delete the user carlos to solve the lab.

## 📖 Key Takeaways
- GraphQL introspection can leak internal API functionality
- Sensitive fields should never be exposed in GraphQL schemas.
- Authorization checks must be enforced server-side for every query.
- Disable introspection in production environments unless absolutely required.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d4079b42-ecea-40fc-891f-2be11ddb439a" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e5dcf207-77fc-4b6c-b77c-89c41fcabb49" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/863a123f-fb51-49c4-b203-8e4773a125e0" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ff959bcd-089f-4a95-9adf-25e562f1898f" />
