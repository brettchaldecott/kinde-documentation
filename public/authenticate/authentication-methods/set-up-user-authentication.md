
Kinde supports many authentication options that let you control how users access your applications.

You can set different authentication requirements for different applications, and also for different organizations (if you use [organizations](/build/organizations/add-and-manage-organizations/)).

Set up all your required authentication methods at the Environment level first.

## Switch on **email authentication**

1. Go to **Settings > Environment > Authentication**.
2. Select **Configure** on the **Email** tile in either the **Passwordless** or **Password** section.
3. In the window that appears, switch the authentication option on or off for each application you have. Note that you **cannot** use passwordless and password authentication for the same app.
4. Select **Save**.

## Switch on phone authentication

You can allow users to authenticate using their phone number as their sign in identity. For full details, see [Set up phone authentication](/authenticate/authentication-methods/phone-authentication/).

1. Go to **Settings > Environment > Authentication**.
2. In the **Passwordless** section, select **Configure** on the **Phone** tile.
3. In the window that appears, switch the authentication option on or off for each application you have.
4. Select **Save**.

## Switch on username + password authentication

You can allow users to authenticate using a username as their sign-in identity. They will still need to provide an email on sign-up, but will be able to sign in with a username-password combination from then on. Learn more about using [usernames for auth](/authenticate/authentication-methods/username-authentication/).

1. Go to **Settings > Environment > Authentication**.
2. In the **Password** section, select **Configure** on the **Username** tile.
3. In the window that appears, switch the authentication option on or off for each application you have.
4. Select **Save**.

## Switch on username + passwordless authentication

You can allow users to authenticate using a username as their sign-in identity. They will still need to provide an email on sign-up, but will be able to sign in with a username-OTP combination from then on. Learn more about using [passwordless auth](/authenticate/authentication-methods/passwordless-authentication/).

1. Go to **Settings > Environment > Authentication**.
2. In the **Passwordless** section, select **Configure** on the **Username + code** tile.
3. In the window that appears, switch the authentication option on or off for each application you have.
4. Select **Save**.

## Switch on **social authentication**

1. Go to **Settings > Environment > Authentication**.
2. In the **Social connections** section, select **Add connection**.
3. In the window that appears, select the social apps you want and then select **Save**.
4. You need to set up the connection to each social app you chose, see [Add social sign in](/authenticate/social-sign-in/add-social-sign-in/).

## Use enterprise or custom authentication

Follow the instructions for the relevant authentication method. See:

- [Microsoft Entra ID](/authenticate/enterprise-connections/azure/) (was Azure AD)
- [SAML](/authenticate/enterprise-connections/custom-saml/)

## Add and manage social and enterprise connections via API

Use [Kinde’s management API](/kinde-apis/management#tag/connections) to manage social and enterprise connections. You can view a list of connections, add a new connection, identify a connection, and update existing connections.
