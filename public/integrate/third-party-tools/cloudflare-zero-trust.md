
If you use Cloudflare to manage authentication across your systems, you can use Kinde as an third-party identity provider.

This topic explains how to set up Cloudflare Zero Trust to use Kinde as an auth identity provider through OpenID Connect.

You need to already have a backend [web application set up](/build/applications/add-and-manage-applications/) in Kinde to follow this procedure.

## Get your Cloudflare team domain

1. [Sign into Cloudflare](https://dash.cloudflare.com/) and navigate to **Zero Trust**.
2. Go to **Settings > Custom Pages**.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/9b73e4d9-5478-44f7-a917-8324c529d100/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

3. Copy your **Team domain**.

## Set up your Kinde app

1. In Kinde, go to **Settings > Applications**.
2. Select **View details** on the relevant backend/web application.
3. Copy the **Client ID** and **Client secret** and add them somewhere you can access later.
4. Scroll to the **Callback URLs** section and enter the Zero Trust Team domain in the **Allowed callback URLs** field. (Copied in the procedure above)

   In this example, we would paste: `mirosaurus.cloudflareaccess.com/cdn-cgi/access/callback`

5. Select **Save**.

## Get your OpenID config info

1. In your browser, go to the OpenID configuration URL of your Kinde business. This will be `https://<your_kinde_subdomain>.kinde.com/.well-known/openid-configuration`

   Our example shows details for `mirosaurus.kinde.com/.well-known/openid-configuration`

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/8e82dc48-a516-42c7-2a41-0bfea682a600/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

2. Copy the following information somewhere you can access it later.
   - jwks_uri - e.g. `https://mirosaurus.kinde.com/.well-known/jwks`
   - token_endpoint - e.g. `https://mirosaurus.kinde.com/oauth2/token`
   - authorization_endpoint - e.g. `https://mirosaurus.kinde.com/oauth2/auth`

## Add Kinde as a provider in Cloudflare Zero Trust

1. Back in the Cloudflare Zero Trust dashboard, go to **Settings > Authentication**.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/eb59f014-9fbb-4ef2-6291-ec1d946a7f00/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

2. In the **Login methods** section, select **Add new**. The **Add a login method** screen opens.
3. Select **OpenID Connect** as the identity provider.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/eb21ed9c-150a-47cd-510d-0a0e5dc15500/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

4. Follow the page guide and enter the following details:
   - Name - Whatever you want
   - App ID - this is the Client ID you copied from your Kinde app
   - Client Secret - this is the Client secret you copied from your Kinde app
   - Auth URL - the `authorization_endpoint` copied in the previous procedure
   - Token URL - the `token_endpoint` copied in the previous procedure
   - Certificate URL - the `jwks_uri` copied in the previous procedure

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/e5cffb1e-e316-473d-d51a-3dac205b1a00/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

5. Select **Save**.

## Enable Cloudflare to use Kinde as an auth provider

1. In the **Zero Trust** dashboard, go to **Access > Applications**.
2. In the **Authentication** tab, select the newly created **Open ID Connect** method.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/7e72598a-d4a6-48ae-7948-c8face8d0200/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

3. Select **Save application**. When an authentication event is triggered, Cloudflare will offload to Kinde to complete the authentication.
