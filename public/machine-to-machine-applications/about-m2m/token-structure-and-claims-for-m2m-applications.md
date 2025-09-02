
Access tokens issued by Kinde to Machine-to-Machine (M2M) applications are JSON Web Tokens (JWTs)that include trusted claims about the app, scopes, and (if applicable) the organization.

This reference explains what those claims are and how to use them securely in your APIs or services.

## How to view M2M token claims

Tokens returned from the client credentials flow can be decoded using any standard JWT library, or online tools like [Kinde's JWT decoder](https://kinde.com/tools/online-jwt-decoder/).

You do **not** need to validate the signature unless you're verifying tokens on your own backend (outside of Kinde-hosted APIs). For most use cases, Kinde validates the token for you when you call our APIs.

## Example M2M token payloads

### Global M2M app

```json
{
  "aud": [
    "your-api-audience
  ],
  "azp": "d4d3c5b74e064badb9625a4aa6241bcc",
  "exp": 1751237068,
  "gty": [
    "client_credentials
  ],
  "iat": 1751150668,
  "iss": "https://<your-subdomain>.kinde.com",
  "jti": "f95ed3e0-cc4d-40c4-b95a-9971729b0ae5",
  "scope": "read:users write:flags",
  "scp": [
    "read:users",
    "write:flags
  ],
  "v": "2"
}
```

### Org-scoped M2M app

```json
{
  "aud": [
    "your-api-audience
  ],
  "azp": "d4d3c5b74e064badb9625a4aa6241bcc",
  "exp": 1751237068,
  "gty": [
    "client_credentials
  ],
  "iat": 1751150668,
  "iss": "https://<your-subdomain>.kinde.com",
  "jti": "f95ed3e0-cc4d-40c4-b95a-9971729b0ae5",
  "org_code": "org_ba4a2311eb1",
  "scope": "read:users write:flags",
  "scp": [
    "read:users",
    "write:flags
  ],
  "v": "2"
}
```

## Standard M2M token claims

| Claim | Description |
|-------|-------------|
| `aud` | The audience for the token. This is the API that the token is intended for. |
| `azp` | The client ID of the M2M app that requested the token. |
| `exp` | The expiration time of the token. |
| `gty` | The grant type for the token. This is always `client_credentials`. |
| `iat` | The issuance time of the token. |
| `iss` | The issuer of the token. This is the Kinde environment URL. |
| `jti` | The unique identifier for the token. |
| `scope` | The scopes granted to the token. |
| `scp` | The list of scopes initially requested |
| `v` | The version of the token. |

## Additional M2M claims for org-scoped apps

| Claim | Description |
|-------|-------------|
| `org_code` | The organization code for the token. |


## Validating and using claims for M2M apps

In your API or backend service, you can use these claims to enforce access:

- Confirm the `aud` matches the expected audience for your API
- If your endpoint is organization-specific (e.g. `/orgs/:org_code/...`), ensure that `org_code` from the token matches the route parameter
- Use `scopes` to implement scope-based access control (e.g. `write:flags` required to enable a feature flag)

## Important information about M2M tokens, audiences, and scopes

- Tokens are signed using asymmetric keys (RS256)
- You can retrieve your Kinde environment’s public keys from the [OpenID configuration endpoint](https://your-subdomain.kinde.com/.well-known/openid-configuration)
- Token claims are added by Kinde based on the M2M app’s configuration and assigned scopes — they cannot be overridden in the token request
- You can request multiple audiences
- You can request specific scopes to limit the permissions of the token
