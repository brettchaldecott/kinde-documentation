
This guide shows you how to create a Machine-to-Machine (M2M) application in Kinde, authorize it for an API, and use the client credentials flow to get a token and make a secure API request.

## Step 1: Create a machine-to-machine app

1. On the Kinde home page, select **Add application**.
2. Enter a name for the application.
3. Choose **Machine-to-machine (M2M)**.
4. Select **Save**.

## Step 2: Create an API in Kinde (if you don't have one)

You can skip this step if you already have an API registered in Kinde.

1. Go to **Settings > APIs**.
2. Select **Add API**.
3. Enter a name for the API.
4. Select **Save**.

For details on the full API setup, see [Register and manage APIs](/developer-tools/your-apis/register-manage-apis/).

## Step 3: Authorize the app to access an API

1. Open the newly created M2M app.
2. Go to **APIs** in the menu.
3. Find the API you want to authorize in the list. 
4. Select the three dots on the far right and select **Authorize**.

## Step 4: (Optional) Add scopes to define permissions

1. Go to **Settings > APIs**.
2. Choose the API you want to protect.
3. Select **Scopes** in the menu and add scopes (e.g. `read:users`, `write:flags`).
4. Once you are finished, open the M2M app and assign the scopes. See [Define and manage API scopes](/developer-tools/your-apis/api-scopes-m2m-applications/).

## Step 5: Get a token to test your M2M app

You can test the app in one of two ways:

### Option A - Use the **Test** details in Kinde

1. Open your M2M app.
2. Go to **Test** in the menu.
3. Select the API (audience).
4. Copy the generated token.

### Option B - Use the client credentials flow directly

```bash
curl --request POST 'https://<your-subdomain>.kinde.com/oauth2/token' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'grant_type=client_credentials' \
  --data-urlencode 'client_id=your-client-id' \
  --data-urlencode 'client_secret=your-client-secret' \
  --data-urlencode 'audience=<your-api-audience>' \
  --data-urlencode 'scope=read:users write:flags'
```

The response will include a bearer token you can use in requests:

```json
{
  "access_token": "<token>",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

## Step 6: Use the token in an API call

Include the token in the `Authorization` header:

```bash
curl https://your-subdomain.kinde.com/v1/organizations \
  -H "Authorization: Bearer <token>
```
