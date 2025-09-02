
<Aside type="upgrade">

For multiple enterprise connections and other advanced organization features, you may need to [upgrade your plan](https://kinde.com/pricing/)

</Aside>

Enterprise connections are common for B2B setups where each business customer is represented as an organization in Kinde, and that organization is linked to one or more connections. There are two different ways to scope an enterprise connection and restrict it to the organization level.

- When the `org_code` is passed to Kinde as part of the authentication url, the correct sign-in options are shown.
- Users can only self-join the organization if this was enabled as part of the connection configuration.
- Organization access is locked down to allow access based only on connection - including switches between organizations.
- If you are using home realm discovery, connections do not have to be enabled at the application level to support redirects to the correct IDP.

This behaviour is domain-agnostic and is purely concerned with the connection being used.

## (Recommended) Create the enterprise connection in the Kinde organization

The easiest way to restrict an enterprise connection to an organization, is to add the connection to the organization and not create it as a shared connection (at the environment level). To do this, follow the relevant procedure for adding a connection in the relevant topic.

## Select a shared enterprise connection for the organization

1. Open the relevant organization in Kinde and select **Authentication** in the menu.
2. Add a connection and select **existing connection**. Switch on the relevant enterprise connection from the list.
3. Select **Save**.

## Org provisioning and access via allowed domains

To manage organization access, you can [set policies](/build/organizations/organization-access-policies/) that restrict access to a list of allowed domains. You can also enable just-in-time (JIT) provisioning via allowed domains.

1. Open the relevant organization in Kinde and select **Policies** in the menu.
2. Select **Allow org members to be auto-added**.
3. Enter all the allowed domains in the **Allowed domains** list.
4. Enable JIT provisioning for all new organization members by selecting **Auto-add users from allowed domains**.
5. Select **Save**.

Here’s what happens:

- When the `org_code` is passed to Kinde as part of the authentication url, the correct sign-in option is shown.
- Kinde checks that users belong to one of the allowed domains before authorizing access.
- The user joins the organization if the domain matches any of the allowed domains.
- Because this check only happens during sign up, you can still separately add users with email domains which fall outside of this restriction. This can be useful if you wish to add contractors or auditors who may have email addresses not in the domain allowlist.

<Aside>

If both enterprise connection and domain restrictions are in place, both checks must be successful.

</Aside>

## Enterprise connections only allow service provider log in, not identity provider log in

If you set up an enterprise connection for a customer using their IdP credentials, they can only sign in to your app via your app, with Kinde as the auth service provider.
The customer cannot sign in to your app via their own connection setup - known as IdP-initiated login.
