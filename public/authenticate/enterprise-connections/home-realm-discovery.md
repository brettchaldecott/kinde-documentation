
Home realm discovery (HRD) provides a seamless sign-in experience for your enterprise auth users. When HRD is configured and a user sign in, Kinde checks which IdP or connection group a user belongs to, before authenticating them. It is also known as Identity Provider or IdP discovery.

When HRD is set up in Kinde, users are authenticated based on the **Home Realm Domain** (email domain) that is entered.

HRD is usually applied where your identity provider (IdP) is a third party, such as Microsoft Entra ID, Google, Cloudflare, etc, and you are using an enterprise or SAML auth setup.

By default, Kinde provides a universal login page where users of any enterprise connection can sign in. They are then silently routed and verified via the relevant IdP.

## How it works

When you set up a [Microsoft Entra ID](/authenticate/enterprise-connections/azure/) or [custom SAML](/authenticate/enterprise-connections/custom-saml/) connection, you’ll configure the home realm (or domains) to be recognized during authentication. All home realm domains must be unique across all connections in the environment.

If HRD is not in place, the end-user must select the relevant log in button to be taken through to the right authentication URL.

When you apply HRD in Kinde, the end-user is recognized and authenticated based on their email domain, without having to select or click anything.

For example, you could configure two different connections as follows:

- Email addresses ending with `enterpriseA.com` use SAML connection A
- Email addresses ending with `enterpriseB.com` use Entra ID connection B

In the back end, the end-user is linked to the correct identity provider via the connection, and they are silently authenticated.

So when Jude Watson arrives at the sign in window and enters `judewatson@enterpriseA.com`, they are routed to the IdP for SAML connection A, and authenticated.

## Showing or hiding the sign in buttons

Even if you have set up HRD, you can choose to show an SSO sign in button so the user has to click to proceed. Learn more [here](/authenticate/enterprise-connections/about-enterprise-connections/#show-or-hide-the-sso-sign-in-button-on-the-auth-page).
