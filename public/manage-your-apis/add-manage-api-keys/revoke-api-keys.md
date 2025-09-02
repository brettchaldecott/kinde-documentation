
Revoking an API key immediately prevents it from authenticating with your APIs. Use this when a key is no longer needed, has the wrong scopes, or is suspected to be exposed.

## How to revoke an API key

You can revoke an API key in the dashboard or [via the API](https://docs.kinde.com/kinde-apis/management/#tag/api-keys/delete/api/v1/api_keys/). Your customers can also revoke keys themselves in the self-serve portal if you have enabled self-serve API keys. See [Self-serve API keys](/manage-your-apis/add-manage-api-keys/self-serve-api-keys/) for more information.

### Revoke a key on behalf of a user or organization in Kinde

- For a user-level key: Go to **Users > [User] > API keys**. Find the key and open the three dots menu > **Revoke**.
- For an organization-level key: Go to **Organizations > [Organization] > API keys**. Find the key and open the three dots menu > **Revoke**.

![Submenu on API key](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/bd8fdfaa-5146-4f11-2c30-a3263ee3e500/public)

## What changes after an API key is revoked?

Verification responses will indicate the key is not usable. Ensure your API enforces this:

```javascript
// After verifying the API key with Kinde
if (!verification.is_valid || verification.status !== "active") {
  return res.status(401).json({error: "Invalid or inactive API key"});
}
```

## Recommendations

- If the key was in use, create a replacement key and distribute it securely.
- Audit logs for usage of the revoked `key_id`.
- Consider rotating other keys if there was a broader incident.
