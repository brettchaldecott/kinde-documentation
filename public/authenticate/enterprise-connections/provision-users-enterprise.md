
When you set up Kinde with enterprise authentication like SAML or Cloudflare, you’ll want to make sure that users are set up with the correct access and identity from day one. How you do this depends on how you ‘provision’ their enterprise user identity.

Users in Kinde are able to have multiple identities to support all the ways they can sign in, such as via email, social sign-in, etc. However, users managed through enterprise connections can only have an enterprise identity.

## Just-in-time (JIT) provisioning for user creation (recommended)

JIT provisioning is the simplest way to add users to Kinde and allow them to authenticate. Rather than importing or pre-provisioning, your users are added to Kinde at the point of their first authentication. 

To enable JIT provisioning, select the **Create a user record in Kinde** option when you set up your enterprise connection.

![option for JIT in the enterprise connection screen](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/74717fdb-d6d2-4f0b-dcf6-2aef95cb7600/public)

The first time the user authenticates, Kinde creates a new user record for them with the identity information passed from your IdP.

## Pre-provision or pre-create users

Sometimes, JIT provisioning is not the right path or may not be possible. For example:

- The user already exists in Kinde and you're switching the auth method to SSO.
- You are importing users from another system and there is existing data related to the user you also wish to import.
- You only want to add a sub-set of users from your directory.

### Add users to Kinde

In all these cases, the users must first exist in Kinde to implement enterprise SSO.

You can add users to Kinde [via import](/manage-users/add-and-edit/import-users-in-bulk/) or [via API](/kinde-apis/management#tag/users/post/api/v1/user). 

All users must have an email address that matches their email with the IdP. This is not necessarily the email identity for sign in, it is purely for initial matching against the IDP provided email.

<Aside title="Users who sign on through multiple enterprise connections">

It’s possible that you manage users who can sign in via multiple enterprise connections. In these cases, the user must have a separate profile for each enterprise connection.

</Aside>

### Provisioning method 1: Assign a connection identity to a user (recommended)

This method of provisioning requires you to add the enterprise connection as part of the user’s identity in Kinde.

**Add the enterprise connection identity via API**

Post identity details to this endpoint `POST /api/v1/users/{user_id}/identities` with `enterprise` as the type and the `connection_id`. For more information, see [Create identity](https://docs.kinde.com/kinde-apis/management/#tag/users/post/api/v1/users/{user_id}/identities).

You can search connections via API and filter them by domain. This can help you obtain the connection ID.

**Add the enterprise connection identity manually**

1. Open a user’s profile and select **Add identity**.
2. In the window that appears, select **Enterprise SSO** as the **Identity type**.
3. Select the relevant **Enterprise connection** from the list.
4. Enter the user’s email as it appears in the identity provider directory.
5. Select **Save**. The user’s profile is updated to show only the enterprise connection identity.

### Provisioning method 2: Set the connection to trust emails from the IDP

A slightly less secure option is to set the enterprise connection to trust emails from your IdP. 

This does save you adding and linking users as per method 1 above, but it also overrides any existing identity information in Kinde (such as email or phone number) with the connection data from the IdP.

To employ this method, select the **Trust email addresses provided by this connection** option in the connection configuration. **Settings > Authentication > Enterprise connections > Configure.**

![Screen shot of trust emails switch](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/92f20fc4-7dcb-4ce1-9fb2-74b4db67e200/public)

When the user signs in with an SSO connection that provides an email that matches the pre-provisioned users email, we will automatically combine the users. Their original email identity will be removed and from this point on they can only authenticate via the SSO connection.

## Troubleshoot SSO issues

**We can’t find your account**

If a user goes to sign in and encounters the ’We can’t find your account’ message, it could be because **Self-joining** for the organization is switched off. This is the right behaviour if you don’t want users without the `org_id` to join the org, but the message is confusing. Switch this on via  **Organization > Policies**.

**Duplicate identities**

If duplicate identities are created for users in Kinde, it may because the **Trust email addresses provided by this connection** option is not switched on in the connection configuration.

**Customer's SSO auth not working**

Some B2B businesses allow their customers to sign in using their own enterprise SSO. When setting this up, a common mistake is that they supply incorrect values for the Client ID and Client Secret, based on the identity provider information. Double check these if you come across connection issues.  
