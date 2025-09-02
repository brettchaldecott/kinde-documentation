
<Aside type="upgrade">

Depending what auth you set up in an organization, you may need to [upgrade your Kinde plan](https://kinde.com/pricing/), for example, for enterprise connections. Charges also apply for each organization that uses [advanced org features](/authenticate/custom-configurations/advanced-organization/).

</Aside>

You can set different authentication methods for each organization you manage in Kinde. You might want to do this if your customers are businesses that require unique auth setups.

## Environment-level and organization-level auth methods

Most authentication methods need to be [set up at the environment level](/authenticate/authentication-methods/set-up-user-authentication/), including social sign in and core mthods such as passwordless, phone auth, etc. These are known as 'Shared connections'. You might also have [organization-level enterprise connections](/authenticate/enterprise-connections/enterprise-connections-b2b/), which are unique to an organization. 

The way each of these connection types is managed, is very similar. 

## Set authentication methods for an organization

This procedure covers adding shared connections, but you can also [add enterprise connections directly to an organization](/authenticate/enterprise-connections/about-enterprise-connections/). 

<Aside type="warning">

When you set authentication for an organization, it completely overrides the auth pattern set at the environment level. Nothing is inherited, except which methods are available to select for the organization.

</Aside>

1. In Kinde, view details of the organization.
2. Select **Authentication** in the menu.
3. Activate the advanced features for this org, if you haven’t already.
4. Select **Add connection**. The **Add connection** window opens.
5. Select **Existing connection**, then select **Next**. A list of all existing connections appears. 
6. Use the switches to enable and disable authentication methods for the organization.
7. When you’ve finished making changes, select **Save**.

## Disable authentication method for an organization

If you remove an auth method for an organization, users can use any remaining methods to authenticate. If you remove all authentication methods, the organization will revert back to using the default auth set up from the environment level.

<Aside type="warning">

Removing an authentication method could cause breaking changes for users of the connection. Make sure users have an alternative method of sign in.

</Aside>

1. In Kinde, view details of the organization.
2. Select **Authentication** in the menu.
3. Select the three dots menu on the connection you want to remove, and select **Disable connection**.
4. A confirmation window opens. 
5. Confirm that you want to disable the connection.

For shared connections, the connection can be easily added back.
For org-level enterprise connection, the connection can be re-enabled.
