# **CCNA Revision Notes: REST API Authentication**

## **1. Authentication Basics**

- **Authentication:** Verifies the identity of a user or system before granting API access.
- Protects application data from unauthorized access or modification.
- Some public APIs do not require authentication.
- API providers may also track usage for analytics and billing.

## **2. Four REST API Authentication Methods**

| Method        | How it works                                        | Key point           |
| ------------- | --------------------------------------------------- | ------------------- |
| **Basic**     | Username and password encoded in Base64             | Not encrypted       |
| **Bearer**    | Uses a token issued by an authorization server      | Token expires       |
| **API Key**   | Uses a static key issued by the API provider        | Valid until revoked |
| **OAuth 2.0** | Grants third-party apps limited access using tokens | Access delegation   |

## **3. Basic Authentication**

- Sends username and password in every API request.
- Uses the HTTP `Authorization` header.
- Credentials are encoded in Base64, **not encrypted**.
- Base64 can easily be decoded.
- Simple to implement, but stolen credentials can provide API access.
- Must use HTTPS.

## **4. Bearer Authentication**

- Uses a **bearer token** instead of sending username/password with every request.
- Client first obtains a token from an authorization server.
- Includes the token in each request:

```
Authorization: Bearer <token>
```

- Tokens usually expire after a set period.
- If stolen, an attacker can use the token until it expires.
- More secure than Basic Authentication, but requires token management.
- Must use HTTPS.

## **5. API Key Authentication**

- Uses a static key issued by the API provider.
- Key is included with each API request.
- Recommended location: HTTP `Authorization` header.
- Can also be sent in a URL or cookie, but URL placement is not recommended because URLs can be logged.
- Useful for tracking individual customers' API usage.
- Common with cloud services and third-party APIs.
- Key does not expire automatically; remains valid until revoked.
- Stolen keys remain usable until revoked.

## **6. OAuth 2.0**

- Framework for **access delegation**.
- Allows third-party applications to access a user's resources without receiving their login credentials.
- Example: "Log in with Google."

**Four parties:**

| Party                | Role                              |
| -------------------- | --------------------------------- |
| Resource owner       | Owns the data/account             |
| Client app           | Third-party app requesting access |
| Authorization server | Issues access tokens              |
| Resource server      | Hosts the protected resource      |

**Basic process:**

1. Client app requests permission from resource owner.
2. Resource owner grants permission.
3. Client exchanges authorization grant for an access token.
4. Authorization server issues the token.
5. Client sends token to resource server.
6. Resource server validates token and provides permitted resources.

- **Access token:** Grants limited access, potentially read-only.
- **Refresh token:** Allows the client to obtain a new access token without requiring the user to log in again.
- Access tokens expire.
- OAuth 2.0 is defined in RFC 6749.

## **7. Quick CCNA Exam Reminders**

- Basic authentication = username + password + Base64.
- Base64 is encoding, **not encryption**.
- Bearer authentication = token issued by an authorization server.
- API key = static key; does not automatically expire.
- OAuth 2.0 = delegated access without sharing user credentials.
- OAuth 2.0 and Bearer authentication involve an authorization server issuing tokens.
- API keys should be sent in the Authorization header, not URLs.
- Use HTTPS with all four methods.

