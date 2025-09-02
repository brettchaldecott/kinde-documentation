
Depending on the complexity of your authentication setup, you or your users may occasionally encounter errors.

Here are some common error codes and troubleshooting steps.

## Sorry we don't see  way to authenticate you

This error message typically appears when there's an issue with your authentication configuration or token handling. A few common causes include:
- Expired or invalid tokens
- Misconfigured authentication settings
- Session management issues
- Incorrect callback URLs

## State not found

The `State not found` error typically occurs when there's a mismatch between your environment variables and the domain you're using during authentication. There may be a trailing space or incorrect syntax.

## Verification code not received (email)

Verification codes are sent almost immediately when triggered. If a code is not received, it might be because:

- Junk/spam folders​ - Email providers and devices may treat OTPs from unknown providers like Kinde as spam. Check your own spam folder, or ask your IT team if emails from Kinde are quarantined by firewalls or other IT defence systems.
- Gmail delays​ - Google Workspace addresses can experience ~4 minute delays due to pre-delivery scanning
- Microsoft Defender filters​ - Aggressive anti-spam filters sometimes quarantine verification emails for Outlook/Hotmail users

If none of the above help troubleshoot the issue, contact Kinde support.

## Verification code not received (mobile device)

Verification codes are sent almost immediately when triggered. If a code is not received, it might be because:

- Network connectivity problems or poor signal strength
- Carrier-level spam filtering blocking OTP messages
- SMS delivery delays during peak usage times
- Full storage preventing new messages from being received
- Phone numbers in certain countries do sometimes experience issues. Contact us if you think this might be the case. 

We recommend using [Twilio](/authenticate/authentication-methods/phone-authentication/), a third-party SMS provider instead of Kinde's default service. This makes troubleshooting issues and accessing logs much easier.

## Error code 578

Description
- Error coming back to Kinde while validating SAML response

Troubleshooting
- Check that the Assertion Consumer Service (ACS) URL is correct on the identity provider

## Error code 780

Description
- The OAuth 2.0 response failed
- The token is valid, but the redirect is null

## Error code 928

Description
- Error getting or reading the connected app configuration

## Error code 1004

Description
- Error getting custom SAML config while initializing SAML redirect

## Error code 1656

Description
- Error with authorization response
- The request is missing a required parameter, includes an invalid parameter value, includes a parameter more than once, or is otherwise malformed

Troubleshooting
- Missing nonce on implicit flow
- Redirect URI might not be with an https
- Only a localhost suffix can be used with http
- Error with workflow

## Error code 1829

Description
- Error exchanging token on connected app callback

Troubleshooting
- Check the redirect URLs for a mismatch
- Check the client credentials

## Error code 1959

Description
- Error decoding SAML response
- Response contains invalid characters for Base64 response

## Error code 3005

Description
- Cannot handle SAML callback
- The userProfile contains invalid values

## Error code 3420

Description
- OAuth 2.0 response failed because token is invalid and redirect ID is null

## Error code 4179

Description
- Handle Social OAuth 2.0 callback - error exchanging token
- Expired secrets for social provider

Troubleshooting
- Check the secret being used with your social provider and ensure that it hasn't expired

## Error code 4617

Description
- Error reading config on OAuth 2.0 callback

Troubleshooting
- Check that the client credentials in both Kinde and IDP are correct
- Check that the redirect and callback URLs in both Kinde and IDP are correct

## Error code 5716

Description
- OAuth 2.0 response failed due to an invalid token
- Redirection was successful

## Error code 6722

Description
- Error configuring SAML provider
This error also appears if a user tries to sign in to your app via their identity provider setup (IdP-initiated) and not the sign in via the enterprise connection in your app, which is supported by Kinde as service provider (SP-initiated).

Troubleshooting
- Check that the settings in the Kinde enterprise connection are correct
- Check enterprise connection metadata URL, entity ID, certificate

## Error code 7558

Description
- SAML callback tokenInfo returned invalid data

## Error code 8030

Description
- Error configuring SAML provider on redirect

Troubleshooting
- Check the enterprise connection metadata URL
- Check that the IDP has the correct ACS URL

## Error code 8809

Description:
- Received browser trust token is different from the one stored in the login session

Troubleshooting
- Start the auth flow again from the sign in or log in button
- The user is trying to start a session in a new tab, browser, or device when there's already a partially completed session in progress
- The user may have bookmarked the auth page when it's partially completed, instead of bookmarking the initial sign in or log in page

## Error code 9055

Description
- Error getting custom SAML provider configuration
- RelayState is invalid or doesn't exist

Troubleshooting
- Check the SAML callback URL
- Check the entity ID
- Check that the SAML IDP is returning a valid RelayState

## Error code 9364

Description
- Error getting authentication request while initializing SAML redirect

Troubleshooting
- Check the enterprise connection private key, certificate, and signature method

## Error code 9697

Description
- Disposable email detected while authenticating a user on sign up in a workflow

## Error code 9881

Description
- Error storing tokens with connected app

Troubleshooting
- Check the refresh token is valid
