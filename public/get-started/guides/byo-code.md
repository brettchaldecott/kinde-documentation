
Once you’ve got a [Kinde business and domain](https://app.kinde.com/register), you’re ready to get started.

You’ve got a couple of options:

- **Fast and easy** - Access the guided **Quick start** via the application tile on the Kinde home page, and follow the in-app instructions for using your own codebase.
- **DIY** - Follow the instructions below.

## 1: Get your SDK

Browse [our SDK documentation](/developer-tools/about/our-sdks/) for the one you want.

SDK documentation contains language-specific instructions for connecting to Kinde. The steps in this document provide an overview of what you need to do.

If we don’t have the language or framework you’re looking for, and you’re an experienced developer, try [using Kinde without an SDK](/developer-tools/about/using-kinde-without-an-sdk/). You can also [request an SDK](https://kinde-21631392.hs-sites.com/en-au/feature-request).

## 2: Add your application in Kinde

Next, you’ll want to connect your code base to the application you set up in Kinde.

Follow these steps to [add a new application to Kinde](/build/applications/add-and-manage-applications/). Kinde also comes with a couple of pre-built applications and you can use those ones if they suit.

Sign in to Kinde and go to **Settings > Applications**.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/98b14309-1d1d-406d-e9c0-c57003f24c00/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

## 3: Get app keys

View details of your application in Kinde and scroll down to [copy the App keys](/get-started/connect/getting-app-keys/). You’ll need to add these to your project’s `.env` file.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/708e61e2-0738-4c51-e376-b675fac52300/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

## 4: Add callback URLs to your Kinde application

While you are viewing the application details, set the callback and redirect URLs for your app.

These define where a user goes when they sign in to you app. You need to set these in order to enable users to sign up.

Enter default localhost details, such as below. Note that `http://localhost:3000` is an example of a commonly used local development URL. It should be replaced with the URL where your app is running.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/39a075f3-ab58-448b-037e-ed154c4a1300/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

You can add other URLs later, when your production environment is ready to go live or you set up a custom domain.

## 5: Add app keys to the .env file

Your code base will include an .env file (or something similar) for storing configuration information. Add the Kinde app details you copied at step 3 to your .env file

Here’s an example from the Next.js app router SDK. You replace `<your_kinde_client_id>`, `<your_kinde_client_secret>`, and `<your_kinde_subdomain>` with the Kinde app details.

```bash
KINDE_CLIENT_ID=<your_kinde_client_id>
KINDE_CLIENT_SECRET=<your_kinde_client_secret>
KINDE_ISSUER_URL=https://<your_kinde_subdomain>.kinde.com

KINDE_SITE_URL=http://localhost:3000
KINDE_POST_LOGOUT_REDIRECT_URL=http://localhost:3000
KINDE_POST_LOGIN_REDIRECT_URL=http://localhost:3000/dashboard
```

## 6: Test user registration

After you complete the previous steps, you should be able to register your first user.

Register your first user by signing up yourself. To view the user, go to the main **Users** page in Kinde.

Continue through the SDK to complete other configuration tasks. Then follow the link in step 7 below for a list of other set up tasks.

## 7: Set up Kinde to work how you want it

Start configuring Kinde to work the way you want by exploring [common set up tasks](/get-started/guides/set-up-tasks/) and customizations. That’s when you’ll really start to build your business on Kinde.
