
Adding authentication to your Bravo Studio app is easier than you think! With **Kinde**, you can let users securely sign up, log in, and access their profile data. This guide will walk you through connecting **Kinde** with **Bravo Studio** so you can build apps that authenticate users seamlessly.

## What you need

- A [Figma account](http://figma.com/) (Sign up for free)
- A [Kinde account](https://kinde.com/) (Sign up for free)
- A [Bravo Studio account](https://www.bravostudio.app/) paid plan (minimum Solo plan)

## Step 1: Set up your Kinde application

1. Sign in to Kinde and select **Add application**.
2. In the dialog that opens, enter a name for the application (we’ve used ‘Bravo Studio app’).
3. Choose **Back-end web** as the application type, and select **Save**.
4. On the **Quickstart** page, select **Other back end**, and select **Save**. 
    
    ![SDK choices](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/a5d21fe1-266e-4dc7-e311-54e787757400/public)
    
5. Go to **Authentication** and switch on each authentication method you want to be available. (e.g. Google, passwordless, etc.) For these to work, you need to [set up the selected auth methods](https://docs.kinde.com/authenticate/about-auth/authentication-methods/) in Kinde. 
6. Select **Save**. 
7. Go to **Details** and note your **Client ID** and **Client secret**. You’ll need these later.
    
    ![App keys in kinde](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/d7cd88fb-2724-415a-d31f-c9f100ab4600/public)
    

## Step 2: Connect Bravo Studio with Kinde

1. Sign in to your [Figma account](http://figma.com/) on the web or desktop.
2. Open the [Bravo Sample: Kinde Auth Starter Kit](https://www.figma.com/community/file/1470830408542459372).
3. Select **Open in Figma**. This will create a copy in your account.
4. Select **Share** in the top-right corner, then select **Copy link** to copy the public Figma file URL.
        
    ![figma share dialog](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/fd4a0c30-8d72-496c-ef2b-ca1aa2be4600/public)
    
5. Sign in to your [Bravo Studio dashboard](https://projects.bravostudio.app/apps), and select **Create a new app**.
6. Paste the Figma file URL and select **Connect Bravoized Figma file.**
    
    ![Convert figma to bravo](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/0abac93d-0f34-48f8-85a5-cc4a545b7100/public)
    
7. In the **App** tab, open the **Home page** screen.
    - Select the **Your Name** text element.
    - Select the **database icon** to configure dynamic data.
    - Choose **Static** and enter `${user.name}`.
    - Bravo supports built-in variables like `${user.id}`, `${user.name}`, and `${user.email}`. [View all built-in variables](https://docs.bravostudio.app/connect-api/request-url-variables/built-in-variables)
    
    ![Shows bravo view of figma](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/36ffe4dc-3e3c-4b2c-ed40-946f4319b900/public)
    
    ![Shows Bravo view of figma continued](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/8de14b7f-ac7b-474c-e6d5-0aae8422cd00/public)
    
8. Go to the **Integrations** tab and enable **OAuth 2.0 code flow**.
    
    ![Bravo login](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/36a186ab-047f-40a9-89eb-b3becfcbc100/public)
    
9. Select **Show** to view your Bravo **callback URLs.** 
10. Copy the URLs.
    
    ![bravo callback urls.png](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/d0228081-a535-47cd-a9e3-a04fd0d41000/public)
    
11. In your **Kinde app > Details** page, paste the **callback URLs** (without trailing `/`) into **Allowed callback URLs** and select **Save**.
    
    ![kinde allowed callback urls.png](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/8760257c-92ed-4f71-91c6-4c26f9b90000/public)
    
12. Build your **Authorize URI:**
    - Find your **OAuth 2.0 URL** in **Kinde > Quick Start** (e.g., `https://<YOUR_BUSINESS>.kinde.com/oauth2/auth`).
    - Find your **Bravo project URI** in the **callback URLs** section.
    - Set **state** to any random 8+ characters.
    - Combine them into the following format.
        
        ```
        https://<YOUR_BUSINESS>.kinde.com/oauth2/auth?response_type=code&client_id=<YOUR_KINDE_CLIENT_ID>&redirect_uri=https://<YOUR_BRAVO_APP_ID>.callbacks.bravostudio.app&scope=openid+profile+email+offline&state=abcxyz123
        ```
        
13. Go back to **Bravo > Integrations** and fill in the following.
    - Client ID: `<YOUR_KINDE_CLIENT_ID>`
    - Client Secret: `<YOUR_KINDE_CLIENT_SECRET>`
    - Authorize URI: The URL you just built
    - Token URI: `https://<YOUR_BUSINESS>.kinde.com/oauth2/token`
    - UserInfo URI: `https://<YOUR_BUSINESS>.kinde.com/oauth2/v2/user_profile`
    - Scope: `openid profile email offline`
        
        ![bravo oauth.png](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/3998a039-7bb0-4dda-424e-f92350323b00/public)
        
14. Select **Save**.
    
    You can find all Kinde endpoints in your business’s OpenID configuration page. (e.g., `https://<YOUR_BUSINESS>.kinde.com/.well-known/openid-configuration`).

## Step 3: Test your Bravo application

1. Download [the Bravo Vision app](https://www.bravostudio.app/download-bravo-vision) on your smartphone.
2. Open **Bravo Vision** and log in with your Bravo account. Your projects will appear.
3. Select **Bravo Sample: Kinde Auth Starter Kit** to open the app preview.
    
    ![sign in entry screen](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/4491108e-d668-45fb-fada-b02760681e00/public)
    
4. Select **Sign in** to register or log in with your Kinde account. Follow the authentication steps.
    
    ![sign in screen](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/e7beda48-18dd-4fa0-18fb-3516804f8d00/public)
    
5. Once authenticated, you’ll be redirected to the app’s homepage. You should see your profile name displayed.
    
    ![enter username](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/8fe71e60-19a5-4a8d-4560-33ac6d0af900/public)
    
That’s it! You’ve successfully connected Kinde authentication with Bravo Studio and tested it on your mobile app.

<Aside title="Troubleshooting">

    If a deployment error occurs, trying changing the Bravo callback URLs in Kinde to be lowercase. See step 11 of **Connect Bravo Studio with Kinde** above. E.g. change `https://a01JMF34GB3VYX7Z5GVQXSGYFP0.callbacks.bravostudio.app`
to `https://a01jmf34gb3vyx7z5gvqxsgyfp0.callbacks.bravostudio.app`. It's odd but it has fixed an issue previously.

</Aside>

## You did it!

Your app can now securely sign in users and display their profile data. This is a huge step towards building production-ready apps with authentication powered by Kinde.

What’s next?

- Explore [Bravo’s API features](https://docs.bravostudio.app/connect-api) to connect your app to live data.
- Dive deeper into Kinde’s [documentation](https://docs.kinde.com/) to unlock more authentication capabilities.
- Start customizing your app’s design and logic in Bravo to make it your own.

Happy building!
