
You can use a Machine-to-Machine (M2M) application in Kinde to request access tokens using the OAuth 2.0 client credentials flow. These tokens can be used to call Kinde’s APIs or your own APIs with no user interaction required.

## Authorize your application

Before an M2M app can request a token for a specific API audience, it must be authorized for that API.

If the app is not authorized for the given audience, the token request will fail.

1. Open the M2M application in Kinde.
2. Select **APIs**.
3. Select the three dots menu and authorize each API the app is allowed to call.
4. Select **Save**.

You can also [authorize apps programmatically](https://docs.kinde.com/kinde-apis/management/#tag/apis/patch/api/v1/apis/{api_id}/applications) using the Kinde Management API.

## Get an access token

Your M2M application will be provided with a `client_id` and `client_secret` which can be used to request a token.

To get a token, make a `POST` request to your Kinde environment’s token endpoint:

```http
POST https://<your-subdomain>.kinde.com/oauth2/token
```

### Required parameters

The request body must include:

```text
grant_type=client_credentials
&client_id=<your-client-id>
&client_secret=<your-client-secret>
&audience=<your-api-audience>
```

If your app has scopes assigned, you can optionally request them:

```text
&scope=read:users write:flags
```

**Note**: The `audience` parameter tells Kinde which API the token is intended for. Use `https://<your-subdomain>.kinde.com/api/v1` when calling Kinde’s management API. If you're protecting your own custom API, the audience should match the identifier you registered for that API in Kinde.

### Example (cURL)

```bash
curl --request POST 'https://your-subdomain.kinde.com/oauth2/token' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'grant_type=client_credentials' \
  --data-urlencode 'client_id=your-client-id' \
  --data-urlencode 'client_secret=your-client-secret' \
  --data-urlencode 'audience=your-api-audience' \
  --data-urlencode 'scope=read:users write:flags'
```

### Successful response

A successful request returns a JSON response with an access token:

```json
{
  "access_token": "<token>",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

## Use the token

Once you have a token, include it as a Bearer token in the `Authorization` header when making API calls:

```http
Authorization: Bearer <token>
```

## Example usage

Calling a Kinde API:

```bash
curl https://your-subdomain.kinde.com/api/v1/organizations \
  -H "Authorization: Bearer <token>
```

## Important information about tokens, audience, claims, and M2M apps

- Access tokens are valid for 1 hour by default.
- The `audience` must match the intended API — tokens are only valid for the audience they’re issued for.
- You can request multiple audiences
- If your M2M app is scoped to an organization, the token will include the `org_code` trusted claim.
- Tokens are JWTs and can be decoded to inspect claims using standard libraries or tools like [Kinde's JWT decoder](https://kinde.com/tools/online-jwt-decoder/).

## Test your M2M app in Kinde

You can also generate a token to help with testing. 

1. Open your M2M app in Kinde.
2. Select *APIs* in the menu, then open the API you want to test with the app.
3. Select the **Test** tab
4. Select **Get token**
5. Copy the access token and use it in your API requests.

This is useful for debugging or verifying scopes and claims without writing code.
