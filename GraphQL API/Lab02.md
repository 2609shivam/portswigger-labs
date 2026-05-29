#  Finding a Hidden GraphQL Endpoint

## 📌 Lab Details
- **Title**: Finding a Hidden GraphQL Endpoint
- **Difficulty**: Practitioner
- **Category**: GraphQL API
- **Lab URL**: https://portswigger.net/academy/labs/launch/3a73e7aa263f5e0db7ac8a1c6cd9ec592206484cee2ac6fde2dc304cb2d13129?referrer=%2fweb-security%2fgraphql%2flab-graphql-find-the-endpoint

## 🔍 Summary
The application exposed a hidden GraphQL endpoint that was not linked anywhere in the UI but still accepted requests.
Although introspection protections were enabled, they relied on weak regex-based filtering that could be bypassed using formatting tricks such as newline injection.
After bypassing introspection defenses, it became possible to enumerate the schema, discover sensitive mutuations, and abuse the API to delete arbitary users.

## 🛠 Steps to Solve
### Find the hidden GraphQL endpoint
1. In Repeater, send requests to some common GraphQL endpoint suffixes and inspect the results.
2. Note that when you send a GET request to `/api` the response contains a "Query not present" error. This hints that there may be a GraphQL endpoint responding to GET requests at this location.
3. Amend the request to contain a universal query. Note that, because the endpoint is responding to GET requests, you need to send the query as a URL parameter.
For example: `/api?query=query{__typename}`.
4. Notice that the response confirms that this is a GraphQL endpoint.

### Overcome the introspection defenses
1. Send a new request with a URL-encoded introspection query as a query parameter.
2. Notice from the response that introspection is disallowed.
3. Modify the query to include a newline character after __schema and resend.
4. Notice that the response now includes full introspection details. This is because the server is configured to exclude queries matching the regex `"__schema{"`, which the query no longer matches even though it is still a valid introspection query.

### Exploit the vulnerability to delete carlos
1. Right-click the request and select **GraphQL > Save GraphQL queries to site map**.
2. Use the `getUser` query to find that the `id` of **carlos** is `3`.
3. In **Target > Site map**, browse the schema again and find the `deleteOrganizationUser` mutation. Notice that this mutation takes a user ID as a parameter.
4. Send the request to Repeater.
5. In Repeater, send a `deleteOrganizationUser` mutation with a user ID of `3` to delete `carlos` and solve the lab.

## 📖 Key Takeaways
- Hidden endpoints are not secure endpoints.
- Regex-based GraphQL filtering is unreliable.
- Introspection should be fully disabled in production environments.
- Even small parser inconsistencies can lead to major security bypasses.

## 🖼️ Screenshot 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2a7bdff6-625f-45ad-879e-ec114a9fc170" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e3f68dfe-ee4f-41e2-991c-2951624131ff" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c9113c7f-41d0-48fe-ac0a-0219255f6ee6" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4d8df1e9-4ab8-4860-aeb9-74d24a6f8cae" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fba13f0f-21ae-4e59-84dc-4e562dfe2798" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/610be61e-224c-4bf8-b7dc-90ece21a1f42" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/edfde19a-87bd-441a-89ca-05746c6674c2" />
