
You can enable users to sign up and sign in using their Microsoft credentials. To enable this, you’ll need a Microsoft Azure account and some developer know-how.

<Aside type="warning" title="Social sign in for production environments">

Before you make your production environment live, you must add your own social app's Client ID and Client secret in the Kinde connection (as per below). Do not use Kinde's app credentials by leaving the fields blank, as this poses a security and performance risk, and makes it difficult to change auth providers without service disruptions for your users.

</Aside>

## **Get the Kinde Callback URL**

1. Sign in to Kinde.
2. Go to the **Settings** page and select **Authentication**.
3. In the **Social connections** section, select **Add connection.**
4. In the window that opens, select **Microsoft**, then select **Next**.
5. In the **Callback URL** section:
   1. If you use Kinde’s domain as your default, copy the **Kinde domain** URL.
   2. If you use custom domains, select the **Use custom domain instead** switch.
   3. If you have only one custom domain, copy the Custom domain URL. If you have custom domains for multiple organizations, select each one from the list and copy the callbacks for each. You need to enter all custom domain callbacks in the Microsoft app.
6. Select **Save**.
7. Use the copied Callback URL to set up the app, see below.

## Register a Microsoft app and set up

1. Go to your account at [https://portal.azure.com/](https://portal.azure.com/).
2. Navigate to **Entra ID**. You can do this from links on the main screen or in the left side menu.
3. Select **Add+ > App registration** or go to **Manage > App registrations > New registration**.
4. Enter a name for the app.
5. Select a **Supported account types option**. In testing, we selected **Accounts in any organizational directory and personal Microsoft accounts**.
6. In the **Redirect URI (optional)** section, select **Web** in the **Select a platform** dropdown.
7. Enter the **Callback URL** from Kinde. The ones you copied in the procedure above. Add additional entries for all your organization custom domain callbacks, e.g. account.customdomainone.com/login/callback, account.customdomaintwo.com/login/callback, etc.
8. Select **Register**. Details of your new app appear.
9. Copy the **Application (client) ID** and paste it in a text file or somewhere you can easily access it again.
10. Select **Certificates and secrets** from the left menu, select + **New client secret.**
11. Enter a name and give it an expiry date (or accept the default), then select **Add**. Details of the secret are generated.
12. Copy the value in the **Value** column and paste it in a text file or somewhere you can easily access it again. Make sure you copy from the **Value** column, not the **Secret ID** column.

## Add Microsoft app credentials to Kinde

1. In Kinde, go to **Settings** > **Authentication**.
2. In the **Social connections** section, on the **Microsoft** tile, select **Configure**.
3. Paste the **Client ID** (Application (client) ID) and **Client secret value** (that you copied from the **Value** column at step 12 above) into the relevant fields.
4. Select if you want to treat this connection as a trusted provider. A [trusted provider](/authenticate/about-auth/identity-and-verification/) is one that guarantees the email they issue is verified. We recommend leaving this off for maximum security.
5. When a user signs in with an email that matches an existing home realm domain (i.e. part of an enterprise connection), you can allow them to sign in using their existing credentials, rather than creating a new identity in Kinde. To make this happen automatically, select the **Auto redirect home realm users** option. 
6. Add any [upstream params](/authenticate/auth-guides/pass-params-idp/) that you want to pass to the IdP.
7. Select which applications will allow Microsoft social SSO.
8. Select **Save**.

Users will now see Microsoft as an option to sign up and sign in to the selected applications.
