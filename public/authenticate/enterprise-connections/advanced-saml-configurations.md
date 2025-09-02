
When you set up a SAML connection, you might need to include advanced configurations to meet identity provider requirements, and to get the connection running properly and securely.

Here's some of the advanced options you will come across when setting up a connection. 

## Name ID

Name ID (Name Identifier) is a key element in a SAML assertion that uniquely identifies the user (subject) within a given SAML context. It is included in the `Subject` element of the SAML assertion and is critical for identifying and linking user identities between your Identity Provider (IdP) and Kinde.

Available Name ID formats:
- **Unspecified**: No particular format is required
- **EmailAddress**: A user is identified by their email address
- **Persistent**: A stable, opaque identifier intended to remain consistent across sessions
- **Transient**: A short-lived identifier, often used in single sign-on (SSO) scenarios for one-time use

The Name ID you select in Kinde must be supported and configured in your IdP.

## Sign request algorithm

The Sign Request Algorithm defines the cryptographic algorithm used to sign SAML requests (AuthnRequest). Signing ensures the authenticity and integrity of SAML messages.

Available algorithms:
- **RSA-SHA256**: A commonly used and secure option.
- **RSA-SHA1**: Older and less secure; often deprecated.

Secure configurations favor SHA256 or stronger algorithms to protect against vulnerabilities.

## Protocol binding

Protocol Binding refers to the transport mechanism used to send the SAML authentication request from Kinde to your IdP.

Common Binding Types:
- **HTTP Redirect Binding**: The SAML request is sent as a URL parameter using a GET request. It is lightweight but limited in message size.
- **HTTP POST Binding**: The SAML request is sent via an HTML form using the POST method. It supports larger payloads and is commonly used for transmitting signed requests.

The choice of binding affects security, performance, and compatibility. POST Binding is generally preferred for secure communications due to its ability to handle signed messages and larger payloads.

## Key attributes

Key Attributes are additional pieces of information about the user that come from your IdP to Kinde. These attributes provide more context about the authenticated user and are often used for access control or personalization.

Kinde-supported key attributes:

- Email Address: The user’s email, often used for identification or communication.
- First Name / Last Name: Used for personalization or internal system mapping.
- User ID: The attribute in the SAML token that contains the user ID.

Only configure key attributes if supported by your IdP.

## Upstream parameters

You can pass provider-specific parameters to an Identity Provider (IdP) during authentication. These are also known as 'upstream params'. The values your pass can either be static per connection or dynamic per user.

You can use upstream params to create a smoother sign in experience - by passing the email through, or to offer an account switcher (such as the Google account switcher) during sign in.

Note that every identity provider has their own set of supported parameters and values, so you'll need to check their documentation to determine which URL parameters are supported.

For more information, see [Pass parameters to identity providers](/authenticate/auth-guides/pass-params-idp/).
