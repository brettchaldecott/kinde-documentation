
Kinde's device authorization flow adheres to `RFC 8628`, also known as the OAuth 2.0 Device Authorization Grant. It enables authorization for devices with limited input capabilities, such as smart TVs, gaming consoles, or IoT devices. Users authenticate on a secondary device (like a phone or computer) while the primary device receives the access token.

## How the device authentication flow works

1. **Device requests authorization**: The device requests a device code and user code from Kinde.
2. **User authenticates**: The user visits a verification URI on another device and enters the user code.
3. **Device polls for token**: The device polls the token endpoint until authorization is complete.
4. **Access granted**: The device receives an access token and can call protected APIs.

## Endpoints for the device authorization flow

### Device authorization endpoint

**URL**: `https://<your-subdomain>.kinde.com/oauth2/device/auth`

**Method**: `POST`

**Content-Type**: `application/x-www-form-urlencoded`

**Parameters**:

- `client_id` (optional): Your application's client ID - can be omitted if you have set an application as the default for device flows
- `audience` (optional): The audience to use for the request

**Response**:

```json
{
  "device_code": "kinde_dc_device_code_here",
  "user_code": "CSLDFDUU",
  "verification_uri": "https://<your-subdomain>.kinde.com/device",
  "verification_uri_complete": "https://<your-subdomain>.kinde.com/device?user_code=CSLDFDUU",
  "expires_in": 600,
  "interval": 5,
  "qr_code": "data:image/png;base64,..."
}
```

### Token endpoint

**URL**: `https://<your-subdomain>.kinde.com/oauth2/token`

**Method**: `POST`

**Content-Type**: `application/x-www-form-urlencoded`

**Parameters**:

- `grant_type`: `urn:ietf:params:oauth:grant-type:device_code`
- `client_id`: Your application's client ID
- `device_code`: The device code received from the authorization endpoint

**Success response**:

```json
{
  "access_token": "eyJ...",
  "expires_in": 86400,
  "scope": "",
  "token_type": "bearer"
}
```

The scope field may be empty because granted scopes are carried in the access token’s scope claim.

**Example error response**:

```json
{
  "error": "authorization_pending",
  "error_description": "The user has not yet completed the authorization"
}
```

## Polling behavior

The device must poll the token endpoint at regular intervals until the user completes authentication:

- **Initial interval**: Use the `interval` value from the device authorization response (typically 5 seconds).
- **Slow down**: If you receive a `slow_down` error, increase the polling interval by 5 seconds.
- **Maximum time**: Stop polling after the `expires_in` time (typically 30 minutes).

## Device authorization flow error codes

| Error Code              | Description                          | Action                         |
| ----------------------- | ------------------------------------ | ------------------------------ |
| `authorization_pending` | User hasn't completed authentication | Continue polling               |
| `slow_down`             | Polling too frequently               | Increase interval by 5 seconds |
| `access_denied`         | User denied the authorization        | Stop polling                   |
| `expired_token`         | Device code has expired              | Request a new device code      |
| `server_error`          | Misconfigured device code            | Request a new device code      |

## Security considerations for device authorization

- **User code format**: User codes are formatted as `XXXXXXXX` for easy entry.
- **Verification URI**: Users should verify they're on the correct domain.
- **Token expiration**: Access tokens expire after 1 hour by default.

## Specifying an audience in a device authorization request

If an `audience` is specified in the request, the access token will include the audience in the `aud` claim. Kinde supports requesting multiple audiences.

The API must be authorized for the device authorization application.

## Scopes and permissions for a device authorization request

If an audience is specified in the request, any scopes which are belong to that audience that are granted to the user by their role will also be granted to the device. The list of scopes will be displayed on the consent screen. If the user consents, the scopes will be included in the `scope` claim of the access token.
