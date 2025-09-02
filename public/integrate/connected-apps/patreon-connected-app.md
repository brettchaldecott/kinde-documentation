
You can connect Patreon to your Kinde account so you can manage Patreon members and campaigns.

Before you can add Patreon as a connected app in Kinde, you need to [set up a machine to machine application](/developer-tools/kinde-api/connect-to-kinde-api/) to connect to Kinde’s API.

This is Step 2 of the [Kinde Connected apps setup](/integrate/connected-apps/add-connected-apps/#step-2-set-up-the-app-you-want-to-connect) topic.

1. Sign up as a creator at [patreon.com](http://patreon.com/).
2. Go to the [Patreon Platform (developer area)](https://www.patreon.com/portal/registration/register-clients) and sign in if you are not already.
3. Select **Create new client**.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/a35a8351-ed96-41ea-2adb-4e2339adb400/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

4. In the window that appears, complete all the required and other fields.
   1. In the **Redirect URI** field, enter the callback URL from your app in Kinde.
   2. In the **Client API Version** field select **2**. This selection defines the included scopes, which are detailed in the [Patreon docs](https://docs.patreon.com/#scopes).
5. When you’re finished, select **Create New Client**. Your new client is shown in the dashboard of the Patreon Portal.
6. Select the down arrow on the client tile. The app details and other client information is shown. Copy the **Client ID and Client Secret**.
7. Next: Finish setting up the connection in Kinde. See [Configure the connected app in Kinde](/integrate/connected-apps/add-connected-apps/#step-3-configure-the-connected-app-in-kinde).
