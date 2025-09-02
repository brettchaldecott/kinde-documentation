
Kinde supports the use of Microsoft Entra ID as an authentication method. We support WS Federated and OAuth2.0 (follow the topic below), and [Microsoft Entra ID SAML](/authenticate/enterprise-connections/entra-id-saml/) which is covered in a separate topic.

If you [import users into Kinde](/manage-users/add-and-edit/import-users-in-bulk/), their Entra ID will be picked up and matched to the relevant connection based on their email address, for a seamless transition to Kinde.

You can also pass [upstream params](/authenticate/auth-guides/pass-params-idp/) to the IdP as part of this procedure.

<Aside>

**Microsoft Entra ID** used to be known as **Microsoft Azure AD**. [More information](https://learn.microsoft.com/en-gb/azure/active-directory/fundamentals/new-name).

</Aside>

## Before you begin

- Register an app in the [Microsoft Entra Admin Center](https://entra.microsoft.com/#home) See the docs [here](https://learn.microsoft.com/en-us/entra/identity-platform/quickstart-register-app/).
- Copy the Client ID and Client Secret from the Microsoft app.
- We recommend you test connections in a non-production environment before activating in a live environment.

## Step 1: Add and configure the connection in Kinde

<Aside>

You can make a connection available only to a specific organization, or you can create it so it can be used across any organization in your business. 

</Aside>

### Add a connection for a specific organization

1. Go to **Organizations** and open the organization. 
2. In the menu, select **Authentication**, then select **Add connection**.
3. In the **Add connection** window, select **New enterprise connection**, then click **Next**.
4. Select the Microsoft connection type you want (WS Federated or OAuth2.0) and then select **Next**.  
5. Next: 'Step 2: Configure the connection'.

### Add a connection that can be shared across multiple organizations

1. Go to **Settings > Environment > Authentication**.
2. Scroll to the **Enterprise connection** section and select **Add connection**. The **Add connection** window opens.
3. Select the Microsoft connection type you want (WS Federated or OAuth2.0) and then select **Save**.
4. Next: 'Step 2: Configure the connection'.

## Step 2: Configure the connection

1. Enter a **Connection name.** Make this something you can easily identify, especially if you are adding multiple connections for different business customers.

   <Aside type="warning">

   If you plan to import users into Kinde, make sure the connection name matches the connection name in the Entra ID record.

   </Aside>

2. Enter your **Microsoft Entra domain.**

   ![Configure screen](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/27b313dc-73c7-4bd5-927f-f8f3d5121800/public)

3. Enter the **Client ID** and **Client secret** as they appear in the MS Entra application. Make sure you use the **Value** of the client secret.
4. Enter **Home realm domains**. This speeds up the sign in process for users of those domains.
   Note that all home realm domains must be unique across all connections in an environment. For more information about how, see [Home realm domains or IdP discovery](/authenticate/enterprise-connections/home-realm-discovery/).
5. If you use home realm domains, the sign in button is hidden on the auth screen by default. To show the SSO button, select the **Always show sign-in button** option. 
6. If you want, select the **Use common endpoint** option. Recommended if you use multi-tenancy.

   ![Provisioning config for entra oauth2](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/c92fad8c-8c81-4481-e891-670a310fe300/public)

7. Select **Extended profile** if you want to sync the additional information stored in a user’s Microsoft profile to their Kinde user profile. Extended attributes data is included in the `extra_claims` object of the access token.
8. If you want to sync user groups, select **Get user groups**. Recommended if you manage permissions and access via user groups in Microsoft. You also need to do some additional setup, see below. 
9. Select if you want to treat this connection as a trusted provider. A [trusted provider](/authenticate/about-auth/identity-and-verification/) is one that guarantees the email they issue is verified. We recommend leaving this off for maximum security.
10. If you want, select **Sync user profiles and attributes on sign in**. Recommended to keep Kinde user profile data in sync with user profile data from Microsoft. If you choose this option, ensure that the global profile sync preference is also switched on in **Settings > Environment > Policies**.
11. If you want to enable just-in-time (JIT) provisioning, select the **Create a user record in Kinde** option. This saves time adding users manually or via API later.
12. Add any [upstream params](/authenticate/auth-guides/pass-params-idp/) that you want to pass to the IdP.
13. Copy the **Callback URL**. You’ll need to enter this in your Entra ID app.
14. Select **Save**.

## Step 3: Add the callback URL to your Entra ID app

1. Open your application in the [Portal](https://entra.microsoft.com/#home).
2. Select the **Redirect URIs** links on the right.
3. Select **Add URI**.
4. In the relevant field, enter your callback URL (from the 'Configure the connection' procedure above)
5. Select **Save**.

## Step 4: Enable the connection in Kinde

Make sure you test the connection before enabling in production for your users.

1. Open the connection configuration page in Kinde.
2. Switch on the connection. This will make it instantly available to users if this is your production environment.
   1. For environment-level connections, scroll down and select the apps that will use the auth method.
   2. For organization-level connections, scroll down and select if you want to switch this on for the org. 
3. Select **Save**.

## (Optional) Sync Entra ID groups with Kinde

### Add groups claim to MS Entra ID app

1. Open your application in the [Portal](https://entra.microsoft.com/#home).
2. Go to **Token configuration** in the left menu. 
3. Select **Add groups claim**.
4. In the window that appears, select the groups to be included in tokens.

![image of edit groups claim screen](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/fed278be-bdcd-43b6-7130-8c866928b700/public)

5. If you want, customize the token properties by type.
6. Save your changes.

For reference, see this Microsoft doc about [configuring optional claims](https://learn.microsoft.com/en-us/entra/identity-platform/optional-claims?tabs=appui/) 

### Customize ID token in Kinde

1. Open your application in Kinde.
2. Go to **Tokens**.
3. Scroll to **Token customization** and select **Configure** on the **ID tokens** tile.
4. Switch on **Social identity** as an additional claim.
5. Select **Save**.

### Access group info in tokens

- ID token - `ext_provider > claims > profile > groups`
- Access token - `ext_groups`

## Step 4: Test the connection

1. Go to your test application and attempt to sign in.
2. If you left the **Home realm domains** field blank in Kinde, when you launch your application, you should see a button to sign in. Click it and go to step 4.
3. If you completed the **Home realm domains** field, you should be redirected immediately to your IdP sign in screen.
4. Enter your IdP details and complete any additional authentication required.
