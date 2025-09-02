
The 'Device Authorization Flow' allows users to authenticate on one device (like a TV or smart device) using another device (like a phone or computer). This is perfect for devices with limited input capabilities.

In this quick start, you'll learn how to implement the device authorization flow using Kinde in just 5 minutes.

## Prerequisites for the device authorization flow

- `curl` or a similar HTTP client

## Step 1: Create a Device Authorization app

1. From the Kinde home page select **Add application**.
2. Enter a name for the application.
3. Choose **Device and IoT**.
4. Select **Save**.
5. Make a note of the Client ID, you'll need this later.

## Step 2: Enable an authentication method for your application

1. Go to **Settings > Authentication**.
2. Select **Configure** on the **Passwordless** > **Email + code** card.
3. Under **Applications** select the application you created in step 1.
4. Select **Save**.

## Step 3: Request a device code

Request a device code from Kinde's authorization endpoint:

```bash
curl -X POST https://<your-subdomain>.kinde.com/oauth2/device/auth \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "client_id=<YOUR_CLIENT_ID>"
```

The response will include a `device_code`, `user_code`, and `verification_uri`:

```json
{
  "device_code": "kinde_dc_...",
  "user_code": "CSLDFDUU",
  "verification_uri": "https://<your-subdomain>.kinde.com/device",
  "verification_uri_complete": "https://<your-subdomain>.kinde.com/device?user_code=CSLDFDUU",
  "expires_in": 600,
  "interval": 5,
  "qr_code": "data:image/png;base64,..."
}
```

## Step 4: Display the user code

Show the `user_code` to the user and provide the `verification_uri_complete` or QR code from the response. The user should:

1. Visit the `verification_uri_complete` URL on their phone or computer.
2. Complete the authentication process.

## Step 5: Poll for the access token

While the user is authenticating, poll the token endpoint:

```bash
curl -X POST https://<your-subdomain>.kinde.com/oauth2/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=urn:ietf:params:oauth:grant-type:device_code" \
  -d "client_id=<YOUR_CLIENT_ID>" \
  -d "device_code=<YOUR_DEVICE_CODE>"
```

Continue polling every 5 seconds (or the `interval` value from the response) until you receive a successful response like:

```json
{
  "access_token": "eyJ...",
  "expires_in": 86400,
  "scope": "",
  "token_type": "bearer"
}
```

## Step 6: Use the access token

Once you have received the access token, you can call your protected APIs:

```bash
curl -X GET https://your-api.com/protected-resource \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## Default app for device flows

When you set up a default app for device flows, this will be the application that is used if no Client ID is specified in the request.

1. Select **Settings** > **Applications**
2. Select the Device Authorization application you want to set as default
3. Select **Set as default**
4. Select **Save**
