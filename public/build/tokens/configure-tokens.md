
Tokens are an essential part of keeping your application secure. They enable the continued verification of users and applications (including APIs), and are a mechanism for detecting unauthorized intruders.

Tokens need to be updated and refreshed to remain secure, which is why you need to set how long a token lasts, for each token type.

## Defining token lifetimes

You can define the lifetime (expiry time) of ID tokens, access tokens, refresh tokens, and SSO session expiry tokens. Expiry and timeouts are usually defined in seconds - where 3,600 seconds is one hour and 86,400 seconds is one day. Tokens and sessions need to be configured per application.

- **ID Tokens**: Contain identity information about a user. These do not need to last long as identity info is only needed at the moment of authentication, and is unrelated to session lifetimes.
- **Access tokens**: Contain access permissions for a user during authentication. These are the most vulnerable token for attacks, and we do not recommend extending the access token lifetime beyond 1 day.
- **Refresh tokens**: Are issued at the same time as an access token, and extend a user's session without them having to reauthenticate. If you want a user to stay authenticated without having to sign in daily or more frequently - set a high lifetime for refresh tokens.
- **Session inactivity timeout**: A user can be requested to re-authenticate if they do not sustain activity in the current session. We recommend setting a fairly short limit  for inactivity - e.g. up to one day. If you extend the session inactivity timeout, a user's data may become vulnerable, for example if they sign in on a public device and forget to sign out. 

Token and session expiry should be approached with priority for system and user security. The aim is to reduce risks such as:
- Token theft through man-in-the-middle attacks
- Unauthorized access through compromised refresh tokens
- Session hijacking on shared or public devices
- Data exposure through prolonged inactive sessions

## **Set token lifetimes**

1. Go to **Settings** **> Environment > Applications.**
2. Select **View details** on the application tile.
3. Select **Tokens** in the side menu.
4. For each token type, set the expiry time in seconds. 3,600 seconds is one hour; 86,400 seconds is one day.
5. Select **Save**.

## Token security

Tokens can be vulnerable to security breaches. Access tokens in particular contain sensitive information, and these tokens can be used to access systems.

Refresh tokens can be used to reduce some of this risk as they can be used to get new access tokens. However, refresh tokens are also a security risk for the same reason they are useful.

To mitigate risk, we recommend using Automatic Reuse Detection and Refresh Token Rotation.

You can revoke access tokens and refresh tokens via the Kinde Management API. [Search the Kinde API docs](/kinde-apis/management/).
