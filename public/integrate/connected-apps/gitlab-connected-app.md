
Before you can add GitHub as a connected app in Kinde, you need to [set up a machine to machine application](/developer-tools/kinde-api/connect-to-kinde-api/) to connect to Kinde’s API.

This is Step 2 of the [Kinde Connected apps setup](/integrate/connected-apps/add-connected-apps/#step-2-set-up-the-app-you-want-to-connect) topic.

1. Sign in to your GitLab account and follow steps 1 to 3 of [these instructions](https://docs.gitlab.com/ee/integration/oauth_provider.html) for adding a group-owned or user-owned application.
2. Name the application.
3. Select the scopes you want to be available in your application. For example, `api`, `read_api`, `read_user`, etc.

   For a scope to be available in Kinde, it needs to be selected here. Note that not every scope visible here will also be available in Kinde. Some scopes have been excluded for security and data protection reasons.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/dcf1eb8b-e83e-4e23-323f-4aac3289a400/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

4. Paste the Kinde callback URL in the **Redirect URI** field.
5. Select **Save**.
6. Copy the **Application ID** and **Secret**, and paste them where you can access them later.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/f3ceadff-02b5-467d-4ade-e22480e87400/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

7. Next: Finish setting up the connection in Kinde. See [Configure the connected app in Kinde](/integrate/connected-apps/add-connected-apps/#step-3-configure-the-connected-app-in-kinde).
