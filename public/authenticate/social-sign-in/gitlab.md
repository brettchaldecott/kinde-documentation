
You can enable users to sign up and sign in using their GitLab credentials. To enable this, you’ll need some technical know-how and a GitLab app and credentials. Here’s some [GitLab docs](https://docs.gitlab.com/ee/integration/oauth_provider.html) that might help.

<Aside type="warning" title="Social sign in for production environments">

Before you make your production environment live, you must add your own social app's Client ID and Client secret in the Kinde connection (as per below). Do not use Kinde's app credentials by leaving the fields blank, as this poses a security and performance risk, and makes it difficult to change auth providers without service disruptions for your users.

</Aside>

## **Get the Kinde callback URL**

1. In Kinde, go to **Settings** > **Authentication**.
2. In the **Social Connections** section, select **Add connection**.
3. In the window that appears, select **GitLab**, then select **Next**.
4. In the **Callback URL** section:
   1. If you use Kinde’s domain as your default, copy the **Kinde domain** URL.
   2. If you use custom domains, select the **Use custom domain instead** switch.
   3. If you have only one custom domain, copy the Custom domain URL. If you have custom domains for multiple organizations, select each one from the list and copy the callbacks for each. You need to enter all custom domain callbacks in the GitLab app.
5. Select **Save**.
6. Use the copied Callback URL to set up the app, see below.

## **Create GitLab app**

1. Sign in to your GitLab account and follow [these instructions](https://docs.gitlab.com/ee/integration/oauth_provider.html) for adding a group-owned or user-owned application.
2. Ensure these scopes are enabled in your application: `read_user`, `openid`, `profile`, `email`.
3. Paste the Kinde callback URL in the **Redirect URI** field. Add additional entries for all your organization custom domain callbacks, e.g. `account.customdomainone.com/login/callback`, `account.customdomaintwo.com/login/callback`, etc.
4. Select **Save**.
5. Copy the **Application ID** and **Secret**, and paste them where you can access them later.

## **Add GitLab credentials to Kinde**

1. In Kinde, go to **Settings** > **Authentication**.
2. On the GitLab tile, select **Configure**.
3. Paste the **Client ID** (**Application ID**) and **Client secret** (**Secret**) into the relevant fields.
4. Select if you want to treat this connection as a trusted provider. A [trusted provider](/authenticate/about-auth/identity-and-verification/) is one that guarantees the email they issue is verified. We recommend leaving this off for maximum security.
5. Add any [upstream params](/authenticate/auth-guides/pass-params-idp/) that you want to pass to the IdP.
6. Select which applications will have a GitLab sign in option.
7. Select **Save**. Users will now see GitLab as an option to sign up and sign in to your product.
