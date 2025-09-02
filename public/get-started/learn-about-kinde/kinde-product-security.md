
Here’s some of the practices, standards, and methods we use to keep your data, and user’s data secure. 

## Auth pages require known origins in the callback URL

To help prevent [XSS attacks](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html), all authentication pages require a [callback URL](/get-started/connect/callback-urls/) to be set in Kinde. Authentication does not work correctly without them. Our application will check the callback URLs and parse out the domain for an origin check.

## Bot detection

Native bot detection is on the roadmap, which will allow customers to bring their own Cloudflare integration key to apply bot detection and protections directly to their Kinde hosted auth pages. This will allow customers to adjust the level of protection based on their own risk profile. Please see our [roadmap](https://updates.kinde.com/board) for updates.

To protect against bot driven volumetric attacks, we use AWS WAF to block traffic that matches OWASP threat rules, known malicious IP addresses, and AWS managed rules. 

Alternatively, customers can implement bot detection and CAPTCHA if your DNS is hosted via Cloudflare. Kinde's hosted pages, with a custom domain, can be proxied through Cloudflare to incorporate their advanced web attack protection features, including bot detection and Cloudflare's Turnstile (Cloudflare's equivalent to CAPTCHA). Please see [Proxy your Kinde auth pages through Cloudflare](/authenticate/custom-configurations/proxy-your-kinde-auth-pages-through-cloudflare/) for more information.

## Credential stuffing protection

Kinde protects from credential stuffing attacks with a combination of techniques. To protect against volumetric attacks, we use AWS WAF to block traffic that matches OWASP threat rules and also use our own application throttling. To protect against password brute forcing, we will automatically lock out accounts after 5 consecutive failed attempts of the password or passwordless code (OTP, password, or multi-factor authentication) for a short period of time. Admins can manually unlock accounts if necessary. Bot detection (see previous section) can be used to provide additional protection.

## CSRF protections via `state` parameter

Kinde protects against [CSRF](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html) attacks by using and recommending the `state` parameter as part of an auth token. All state changing requests generate a CSRF tokens server side, which are validated on the backed, and cookies are set to sameSite `strict`.

## Hosted auth pages prevent clickjacking

To prevent [clickjacking](https://cheatsheetseries.owasp.org/cheatsheets/Clickjacking_Defense_Cheat_Sheet.html) attacks where a user’s personal information or credentials may be stolen by hidden iFrames, use Kinde’s hosted pages for authentication. Hosted pages have protections in place such as such as strict CSP and security headers to prevent itself from being embedded as an iFrame.

Kinde controls and hosts the authentication pages, so the risk for protecting pages is assumed by Kinde rather than you.

## Enforced password standards

Kinde provides a baseline password protection policy for any customer who wants to include password authentication in their product.

- 8 character minimum
- No complexity requirements
- Blocking of 1,000,000 most common passwords
- 5 incorrect attempts locks account out for 5 minutes

Note that Kinde recommends, where possible, not using passwords for authentication and instead using [passwordless](/authenticate/about-auth/authentication-methods/#passwordless) or [social](/authenticate/social-sign-in/add-social-sign-in/) integrations. If using passwords for authentication, we recommend adding [multi-factor authentication](/authenticate/multi-factor-auth/about-multi-factor-authentication/) as an option for users.

## Password security

If your business is set up so users sign in with passwords, you can be assured that Kinde uses a hashing algorithm and never stores passwords as text. Specifically, we use bcrypt for hashing of passwords in our database.

## Denial of service mitigation via rate limits and throttles

Kinde has implemented an application level rate limit and throttle. This helps protect our shared infrastructure and our customers from excessive network traffic that could lead to an outage. Our rate limits are tuned based on the type of traffic, such as API, admin pages, or general auth usage, and will limit the number of connections from an IP in a short space of time. This generally covers all interactions for our service.

## Device fingerprinting prevents session hijacking

Kinde implements a method of device fingerprinting during the authentication flow to block the transfer of sessions between different browsers or devices that could lead to [session hijacking](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html).

## Enforcement of TLS version and ciphers

Kinde enforces strict [TLS versions](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html) and ciphers across it’s public facing websites and API endpoints.

TLS 1.3

- TLS_AES_128_GCM_SHA256
- TLS_AES_256_GCM_SHA384
- TLS_CHACHA20_POLY1305_SHA256

TLS 1.2

- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384

## Preventing third-party script injections with XSS and CSP

Kinde uses various techniques to help prevent the execution of unknown third party scripts and the injection of malicious third party scripts by using XSS protection and CSP.

Kinde’s server rendered application encodes output by default, which prevents most opportunities to inject HTML. The Content Security Policy configured across our webpages prevents the execution of inline and third party scripts.

## Blocking malicious traffic with our WAF

Kinde has implemented a web application firewall ([WAF](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Cloud_Architecture_Cheat_Sheet.html#web-application-firewall)) across all of our public-facing websites and API endpoints. We use AWS WAF to block traffic that matches OWASP threat rules, known malicious IP addresses, and AWS managed rules. 
