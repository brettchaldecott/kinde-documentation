
Enterprise authentication is a common method for managing user access to systems in large organizations.

Kinde supports a number of enterprise connection types, including:

- [Custom SAML](/authenticate/enterprise-connections/custom-saml/)
- [Microsoft Entra ID](/authenticate/enterprise-connections/azure/) (was Azure AD) WS Federated or Open ID
- [Microsoft Entra ID (SAML)](/authenticate/enterprise-connections/entra-id-saml/)
- [Google Workspace](/authenticate/enterprise-connections/custom-saml-google-workspace/) (via SAML)
- [Okta](/authenticate/enterprise-connections/okta-saml-connection/) (via SAML)
- [Cloudflare](/authenticate/enterprise-connections/cloudflare-saml/) (via SAML)
- [LastPass](/authenticate/enterprise-connections/lastpass-sso/) (via SAML)

<Aside type="upgrade">

The number of enterprise connections you can have depends on your [Kinde plan](https://kinde.com/pricing/).

</Aside>

## Provisioning for enterprise connections

Kinde offer a number of provisioning options for enterprise connections, including **just in time (JIT)** provisioning and **pre-provisioning** options.

See [Provisioning users with enterprise connections](/authenticate/enterprise-connections/provision-users-enterprise/)

## How identities are handled in enterprise connections

Users with enterprise identities in Kinde can’t also have other identity types in Kinde. E.g. a user can have an email identity and a social identity. But if a user has an enterprise identity, they cannot have other identities. In this case, identity information is sourced with the identity provider (IdP) and is managed via the identity provider, not in Kinde. Learn more about [identities in Kinde](/authenticate/about-auth/identity-and-verification/).

## Enterprise connections for B2B businesses

Many businesses have businesses for customers (B2B), and use Kinde organizations to manage authentication and access. Kinde lets you set a number of enterprise authentication features at the organization level, see [Enterprise authentication for B2B](/authenticate/enterprise-connections/enterprise-connections-b2b/).

## Session sign out behavior 

When enterprise connection users sign out, they are only signed out of the Kinde session, they are not signed out of the identity provider. We do not force sign out of the IdP because this could break existing sessions the user is signed into, for other applications. This behavior also applies for social connections, where a third party is the identity provider.

## Routing in enterprise connections

When users sign up via an enterprise connection with single-sign-on (SSO), they are routed to the identity provider (IdP) for identity verification. This happens when they select the SSO button on the home screen. You can set up a more seamless routing option using home realm discovery.

### Home realm discovery

Home realm discovery routes users based on their email domain. So when a user enters their email and selects the continue button, they are routed to their IdP based on the email domain, to authenticate. For example if the user enters [chris@acme.com](mailto:chris@acme.com) Kinde checks which IdP uses the [**acme.com**](http://acme.com/) domain and silently verifies his identity. He only signs in once.

Note that this feature has nothing to do with security or access control and everything to do with routing. Not to be confused with setting access restrictions for [domain allowlists](/authenticate/enterprise-connections/enterprise-connections-b2b/).

Learn more about [home realm discovery](/authenticate/enterprise-connections/home-realm-discovery/).

## Show or hide the SSO sign-in button on the auth page

When you set up enterprise auth in Kinde, an SSO button appears on the authentication page which is linked to the IdP by default. Users can select this as a sign up method, similar to how they might select a Google or Facebook sign-in option. For a more seamless experience, you can hide the SSO button by entering a home realm domain for the connection (more info above). Users will be routed silently via their IdP when they enter their credentials.

If you have multiple enterprise auth methods (E.g. SAML and Entra ID), you may not want to show multiple SSO buttons. Here's the options for showing and hiding, depending how many enterprise auth methods you add:

### (Option 1) Hide all SSO buttons

If you configure home realm discovery in each enterprise auth method, all SSO buttons will be hidden by default. The user enters their credentials and they are silently authenticated against the relevant IdP based on email domain.

### (Option 2) Show a universal SSO button for all

If you would prefer users explicitly choose to sign in with SSO, you can add a universal button to the sign in screen. 

1. Go to **Settings > Applications > Your application**.
2. On the **Details** page scroll down to the **Authentication experience** section. 
3. Switch on **Show 'Sign in with SSO' button**.

Users click the universal button, enter their credentials, and get routed silently to the IdP for verification. 

## Enterprise connections only allow service provider log in, not identity provider log in

If you run a B2B business, you might allow your business customers to use their own identity provider setup (like Okta SAML) to access your app. When you set up an enterprise connection to support this, make them aware they can only sign in via your app's auth gateway, with Kinde as the auth service provider.
The customer cannot sign in to your app via their own connection setup - also known as IdP-initiated login.

## Disable an enterprise connection

<Aside type="danger">

Before you disable a connection, make sure that there are no users relying on it for authentication. Once disabled, the sign in option becomes unavailable to users.

</Aside>

1. Navigate to the connection in Kinde. Via **Organization > Authentication** or via **Settings > Authentication**.
2. For an organization-level connection:
   1. Select the three dots menu on the connection tile. 
   2. Select **Disable connection**. 
   3. Confirm the action in the confirmation window.
3. For an enterprise level connection:
   1. Select **Configure** on the connection tile.
   2. Scroll down and disable the connection for each application.
   3. Select **Save**. Confirm the action in the confirmation message.

## Delete an enterprise connection

<Aside type="danger">

Before you delete a connection, make sure that there are no users relying on it for authentication. Once deleted, the sign in option becomes unavailable to users. This action can’t be reversed.

</Aside>

1. Navigate to the connection in Kinde. Via **Organization > Authentication** or via **Settings > Authentication**.
2. Select the three dots menu on the connection tile.
3. Select **Delete connection**. 
4. Confirm the action in the confirmation window.
