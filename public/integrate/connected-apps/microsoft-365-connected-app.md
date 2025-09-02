
You can now set up a connection between Kinde and Microsoft 365. You might do this, for example, to request Microsoft 365-related permissions from users during auth. 

The procedure below covers the case for adding calendar permissions.

## Before you begin

- Set up a [machine to machine application](/developer-tools/kinde-api/connect-to-kinde-api/)
- Ensure you have at least one [end-user created](/manage-users/add-and-edit/add-and-edit-users/)
- Complete [Step 1: Add a connected app to Kinde](/integrate/connected-apps/add-connected-apps/#step-1-add-the-connected-app-in-kinde)

You also need a [Microsoft Azure account](https://portal.azure.com/#home) for creating MS Entra ID apps.

## Register a Microsoft app

This is Step 2 of [Add a connected app to Kinde](/integrate/connected-apps/add-connected-apps/#step-1-add-the-connected-app-in-kinde).

1. Sign in to your [Microsoft Azure account](https://portal.azure.com/).
2. Navigate to **Entra ID**. You can do this from links on the main screen or in the left side menu.
3. Select **Add+ > App registration** or go to **Manage > App registrations > New registration**.
4. Enter a name for the app.
5. Select a **Supported account types option**. For testing purposes, we selected **Accounts in any organizational directory and personal Microsoft accounts**.
6. In the **Redirect URI (optional)** section, select **Web** in the **Select a platform** dropdown.
7. Enter the **Provider to Kinde callback URL** from the Kinde connected app. For example, `https://app.<yourdomain>/connected_apps/callback`.
8. Select **Register**. Details of your new app appear.
9. Copy the **Application (client) ID** and paste it in a text file or somewhere you can easily access it again. You’ll use this as the **Client ID** for the connected app in Kinde.

## Add calendar scopes and generate a secret

<Aside type="warning">

While it may be possible to add other scopes, we have currently only tested the inclusion of calendar scopes.

</Aside>

1. Add scopes to the Microsoft app. 
    1. Go to **API permissions**.
    2. Select **+ Add a permission**.
    3. In the pop-out, select **Microsoft Graph** and then **Application permissions**.
    4. Search for ‘calendar’, then select only `Calendars.Read` and `Calendars.ReadWrite`.
        
        ![Entra app Permissions for calendar](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/1fa72783-a7e0-45d1-08ee-ba6df5106300/public)
        
    5. Select **Add permissions**. These permissions can now be selected as scopes in the Kinde connected app.
2. Generate a secret.
    1. Select **Certificates and secrets** from the left menu, select + **New client secret.**
    2. Enter a name and give it an expiry date (or accept the default), then select **Add**. Details of the secret are generated.
    3. Copy the value in the **Value** column and paste it in a text file or somewhere you can easily access it again. Make sure you copy from the **Value** column, not the **Secret ID** column.
        
        ![Entra app secret value not secret ID](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/56dd1ace-8646-44c9-9add-982528e4a800/public)
        

## Finish configuring the connection in Kinde

You’ll need the **Application (Client) ID** and **Secret** that you copied above. You’ll also need your Kinde app callback URL.

See [Step 3: Configure the connected app in Kinde](/integrate/connected-apps/add-connected-apps/#step-3-configure-the-connected-app-in-kinde).
