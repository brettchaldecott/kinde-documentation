
You can enable users to sign up and sign in using their Bitbucket credentials. To enable this, follow all the steps below to create a Bitbucket app and configure credentials in Kinde.

<Aside type="warning" title="Social sign in for production environments">

Before you make your production environment live, you must add your own social app's Client ID and Client secret in the Kinde connection (as per below). Do not use Kinde's app credentials by leaving the fields blank, as this poses a security and performance risk, and makes it difficult to change auth providers without service disruptions for your users.

</Aside>

## **Get your Kinde callback URL**

1. In Kinde, go to **Settings** > **Authentication**.
2. In the **Social connections** section, select **Add connection.**
3. In the window that appears, select **Bitbucket,** then select **Next**. 
4. In the **Callback URL** section:
   1. If you use Kinde’s domain as your default, copy the **Kinde domain** URL.
   2. If you use custom domains, select the Use custom domain instead switch.
   3. If you have only one custom domain, copy the Custom domain URL. If you have custom domains for multiple organizations, select each one from the list and copy the callbacks for each. You need to enter all custom domain callbacks in the Bitbucket app.
5. Select **Save**.
6. Use the copied Callback URL to set up the app, see below.

## **Create and configure a Bitbucket app**

1. Create an account on [https://bitbucket.org/](https://bitbucket.org/).
2. Go to **Workspaces** if you are not automatically directed.
3. Create a workspace.
4. Enter a **Name** and **ID**.
5. Open **Settings** > **Apps and features** > **oauth consumers.**
6. Add a consumer.
7. Enter a name and add the callback URLs copied from your Kinde app. Add entries for all your organization custom domain callbacks, e.g. `account.customdomainone.com/login/callback`, `account.customdomaintwo.com/login/callback`, etc.
8. Under **Permissions**, in the **Account** section, select **email** and **read.**
9. Select **Save**.
10. Go back to **Apps and features** > **oauth consumers.**
11. Select the workspace you’ve just created and copy the key (client id) and secret (client secret) to paste into the kinde app.

## **Add Bitbucket app credentials to Kinde**

1. In Kinde, go to **Settings** > **Authentication**.
2. On the **Bitbucket** tile, select **Configure**.
3. Paste the **Client ID** (key) and **Client secret** (secret) into the relevant fields.
4. Select if you want to treat this connection as a trusted provider. A [trusted provider](/authenticate/about-auth/identity-and-verification/) is one that guarantees the email they issue is verified. We recommend leaving this off for maximum security.
5. Add any [upstream params](/authenticate/auth-guides/pass-params-idp/) that you want to pass to the IdP.
6. Select which applications will allow Bitbucket social sign in.
7. Select **Save**.

Users will now see Bitbucket as an option to sign up and sign in to the selected applications.
