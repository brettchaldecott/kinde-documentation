
During authentication, ID tokens carry information about authenticated users securely to your application.

## ID token standard claims

- **At hash** - `at_hash` ensures the integrity of the claim made in the access token.
- **Audience** - intended recipient of the token. Represented as the token's `aud` claim. This could be your kinde domain or app URI, e.g. `https://<your_subdomain>.kinde.com`
- **Authentication time** - `auth_time` indicates the time when the user was authenticated. It's useful for scenarios where certain actions are allowed only if the user has recently authenticated.
- **Authorized party** - an `azp` claim specifies the client ID of the party to which the ID Token was originally issued.
- **Email** - the `email` associated with the user’s profile
- **Expiration Time** - The `exp` claim specifies the timestamp when the ID token expires and should no longer be considered valid. It helps prevent the token from being used indefinitely. More about [setting token expiry in Kinde](/build/tokens/configure-tokens/).
- **Issued At** - The `iat` claim indicates the timestamp when the ID token was issued. It can be used to determine the token's age and to mitigate replay attacks.
- **Issuer** - The `iss` claim specifies the issuer of the ID token, usually the URL of the authorization server or identity provider. It's used to verify the token's authenticity.
- **Picture URL** - the `picture` claim contains the location reference of the avatar picture of the user, if there is one.
- **Subject -** The `sub` claim is a unique identifier for the authenticated user within the context of the issuing authentication server. In Kinde, this is the user’s ID.
- **Token ID** - the `jti` claim is the unique identifier of the ID token
- **Updated at** - the `updated_at` claim specifies the issuer of the ID token, usually the URL of the authorization server or identity provider. It's used to verify the token's authenticity.
- **User last name** - the `family_name` claim contains the user’s last name
- **User first name** - the `given_name` claim contains the user’s first name
- **User full name** - `name` contains the first name and last name of the user

## Kinde additional claims

- **Social identity** - Details from the user’s third-party profile, such as handle, username, and ID.
- **Organizations** - The `org_codes` claim contains an array of IDs for the Kinde organizations that the user belongs to.

## Example ID token

```jsx
{
  "at_hash": "VZ6cU0Ay0RKB5EosbWuTCQ",
  "aud": [
    "https://<your_subdomain>.kinde.com
  ],
  "auth_time": 1692361334,
  "azp": "dee7f3c57b3c47e8b96edde2c7ecab7d",
  "email": "jane.smith@gmail.com",
  "exp": 1693288799,
  "family_name": "Smith",
  "given_name": "Jane",
  "iat": 1693285199,
  "iss": "https://<your_subdomain>.kinde.com",
  "jti": "fcxf6xd3-8c75-402x-a4cb-1659fb8c555d",
  "name": "Jane Smith",
  "org_codes": [
    "org_xxxxxxxxxxx
  ],
  "picture": "https://lh3.googleusercontent.com/a/google-url",
  "provided_id": "<user_id_in_your_system>",
  "sub": "kp_xxxxxxxxxxxxxxxxxxxx",
  "updated_at": 1692009540
}
```

## Can't find a claim in the token?

Missing token claims are usually caused by missing scope requests in your app. If you are not using an SDK, you need to manually add scopes (such as `profile`, `email`, `openid`) so that the token you receive from Kinde includes the right claims. Review this document if you are [not using an SDK](/developer-tools/about/using-kinde-without-an-sdk/).
