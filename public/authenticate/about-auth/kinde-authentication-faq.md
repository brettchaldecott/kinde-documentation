
Here’s some short answers to the most common authentication questions. Click on any question to expand the answer.

<Aside title="Ask the docs">

Type **@kindeAI** to access 24/7 global support in the **#ask-kinde-ai** channel in the Kinde [Slack](https://join.slack.com/t/thekindecommunity/shared_invite/zt-1vyq8qilj-jFH5V27jfFnHk~BuBSU0ZA) or [Discord](https://discord.gg/KdkCXRNTFn) communities.

</Aside>

## Authentication methods & core functionality

<details>
<summary><strong>How do I choose the right Kinde authentication method for my SaaS product's user base?</strong></summary>

Think about who you're building for and what they're used to. If you're targeting developers, they'll probably expect GitHub or GitLab sign-in options. Building something for everyday consumers? Google and Apple sign-in will feel familiar and remove friction. Got enterprise customers? They'll likely need SAML or Microsoft Entra ID to keep their IT teams happy. 

You don't need to figure it all out upfront. Start simple with email authentication and add more options as you grow. Your web app and mobile app can even have different auth requirements if that makes sense for your users.
[Learn more about authentication methods](/authenticate/about-auth/about-authentication/)
</details>

<details>
<summary><strong>What's the difference between Kinde's password and passwordless authentication, and which should I implement?</strong></summary>

With passwords, your users create and remember their own passwords (we enforce 8+ characters and block common ones). With passwordless, we send them a one-time code via email or SMS instead. Passwordless is genuinely more secure - no passwords to store, steal, or forget, and codes expire after 2 hours. Just know you can't mix both methods for the same app - it's one or the other. 

If you're building something mobile-first or want to reduce support tickets about forgotten passwords, passwordless is your friend.
[Password authentication guide](/authenticate/authentication-methods/password-authentication/) | [Passwordless authentication guide](/authenticate/authentication-methods/passwordless-authentication/)
</details>

<details>
<summary><strong>How does Kinde's automatic account linking work?</strong></summary>

Kinde automatically connect accounts when someone uses the same verified email across different sign-in methods. So if someone first signs up with Google, then later tries to create an account with Slack using the same email, we'll link them together automatically. No duplicate accounts, no confusion. They can sign in with either method to access the same account. 

Once your users verify their email with any method, all their sign-in options with that email will work seamlessly together.
[Learn about identity and verification](/authenticate/about-auth/identity-and-verification/)
</details>

<details>
<summary><strong>Can I set different Kinde authentication requirements for different parts of my application?</strong></summary>

Absolutely! This is one of those features that sounds complicated but actually makes total sense. Set up your baseline authentication methods at the environment level, then customize per application or organization as needed. Maybe your main web app uses social sign-in to keep things simple, while your mobile app needs phone authentication, or you want business customers to use MFA while keeping consumer sign-up super easy. 

It's all about matching the auth experience to what makes sense for each user group.
[Configure authentication options](/authenticate/authentication-methods/set-up-user-authentication/)
</details>

<details>
<summary><strong>How do I handle Kinde authentication across multiple domains and subdomains?</strong></summary>

We've got this covered with multi-domain authentication. Think about how Google lets you stay signed in whether you're on Gmail, Calendar, or Drive - same idea. Users can hop between `website.yourapp.com`, `app.yourapp.com`, `docs.yourapp.com` and stay authenticated. The trick is using `prompt=none` in your auth URL, which invisibly checks for an existing session. If they're already signed in elsewhere, it's seamless. If not, they'll get prompted to sign in again.
[About Kinde authentication](/authenticate/about-auth/about-authentication/)
</details>

<details>
<summary><strong>What happens when my users can't receive their Kinde auth verification codes?</strong></summary>

Tell them to check their spam folder first. This fixes it most of the time because email providers can be overly protective. For SMS codes, make sure they've got decent cell reception. If they're taking too long (codes expire after 2 hours), they'll need to request a fresh one. And if you're using SMS auth, double-check that your Twilio setup is working properly - that's usually where things go sideways.
[Passwordless authentication troubleshooting](/authenticate/authentication-methods/passwordless-authentication/)
</details>

## Social sign-in

<details>
<summary><strong>How do I set up Kinde social authentication without compromising security for my production app?</strong></summary>

Before you go live, you absolutely must swap out Kinde's default social credentials with your own. We let you use ours for testing, but keeping them in production is asking for trouble - security risks, performance issues, and you'll be stuck if you ever want to switch providers. 

Create your own apps with Google, GitHub, Apple, whatever you're using, grab your Client ID and Client Secret, and put them into Kinde's social connection settings. Don't forget to add your custom domain callbacks if you're using those. [Add and manage social connections](/authenticate/social-sign-in/add-social-sign-in/)
</details>

<details>
<summary><strong>Some social providers don't provide email addresses. How does Kinde maintain these user identities?</strong></summary>

Some social providers (looking at you, X/Twitter and Apple) don't always hand over email addresses, but we need them for security things like password resets. When this happens, we'll ask your users for an email address just once during their first sign-up. After that one-time thing, they can sign in with their social account without any hassle. It's a small bump in the road that keeps everyone secure.
[X social sign-in](/authenticate/social-sign-in/twitter/) | [Apple social sign-in](/authenticate/social-sign-in/apple/)
</details>

<details>
<summary><strong>How can I use Kinde to create a seamless social authentication experience for my users?</strong></summary>

If you're going all-in on social auth (no email/password fallbacks), you can create a pretty slick experience. Users click your sign-in button and boom - straight to Google's or Apple's account picker. Set up custom authentication pages and use the `connectionId` parameter to skip our initial screens entirely.

The only catch? We'll still handle verification and MFA screens because, well, security matters and we're good at it.
[Custom authentication pages](/authenticate/custom-configurations/custom-authentication-pages/)
</details>

<details>
<summary><strong>Should I mark social connections as "trusted providers" in Kinde?</strong></summary>

Generally, no - leave this off for better security. "Trusted provider" means we'll take their word that emails are verified, but here's the thing: people change email addresses, and social providers don't always keep up. Only flip this switch if you're 100% certain the provider maintains verified, current email addresses. When in doubt, err on the side of caution.
[Social connections configuration](/authenticate/social-sign-in/add-social-sign-in/)
</details>

## Enterprise authentication & SAML

<details>
<summary><strong>How do I set up Kinde SAML authentication for enterprise customers?</strong></summary>

SAML setup is where things get a bit technical, but stick with us. Kinde acts as the service provider while your enterprise customer brings their own identity provider (Google Workspace, Microsoft Entra ID, Cloudflare, whatever they're using). You'll create an enterprise connection in Kinde, make up a unique Entity ID (just a random string like "870sa9fbasfasdas23aghkhc12zasfnasd"), and get their IdP metadata URL. They'll need to add your ACS URL to their setup. 

Pro tip: generate certificate and private key pairs for extra security, and always test in a sandbox environment first.
[Custom SAML setup](/authenticate/enterprise-connections/custom-saml/)
</details>

<details>
<summary><strong>What do I use home realm domains for in Kinde enterprise connections?</strong></summary>

Home realm domains are basically a shortcut that makes enterprise sign-in smoother. When you set `bigcorp.com` as a home realm domain, anyone with a "@bigcorp.com" email gets automatically routed to their company's sign-in flow - no extra clicks needed. Just remember that each domain can only be used once across all your connections, so no sharing. And the SSO button disappears by default when you use this (though you can bring it back if needed).
[Microsoft Entra ID setup](/authenticate/enterprise-connections/azure/)
</details>

<details>
<summary><strong>What's the best way to migrate enterprise users to Kinde?</strong></summary>

Get your enterprise connections set up in Kinde first - SAML, Entra ID, whatever they're using. Then when you import their user data (via CSV or JSON), we'll automatically match everyone to the right connection based on their email addresses. This means their sign-in experience stays exactly the same - they won't even notice you've switched to Kinde behind the scenes. Import their roles and permissions too if you've got them.

[Migrate to Kinde](/get-started/switch-to-kinde/switch-to-kinde-for-user-authentication/)
</details>

<details>
<summary><strong>How do I handle enterprise users who are already signed into their identity provider with Kinde?</strong></summary>

When enterprise users sign out of your app, they're only signing out of Kinde, not their company's identity provider (like Entra ID). This is totally normal for federated auth - it's how most enterprise setups work. If your customer really needs full sign-out from both systems, you'll need to build additional logout flows, but honestly, most companies prefer it this way.
[Entra ID SAML connection](/authenticate/enterprise-connections/entra-id-saml/)
</details>

## Multi-factor authentication

<details>
<summary><strong>How do I use Kinde to implement MFA for different types of customers?</strong></summary>

MFA is one of those things where one size definitely doesn't fit all. You can set it up for everyone (environment level) or get granular with specific customer segments (organization level). Finance and government customers? They'll probably want mandatory MFA. Consumer-facing app? Maybe make it optional so you don't scare people away. 

Kinde supports email codes, SMS codes, and authenticator apps as second factors. Just don't use the same method twice - as in don't make email the primary and secondary auth method. That could be confusing. [Enable multi-factor authentication](/authenticate/multi-factor-auth/enable-multi-factor-authentication/)
</details>

<details>
<summary><strong>Can I exempt certain users or connections from MFA requirements in Kinde?</strong></summary>

Yep, you've got options here. You can exempt specific roles (maybe only admins need MFA) or exempt enterprise connections where MFA is already handled by their company's identity provider. Nobody wants double MFA - that's just annoying. Set these exemptions at the organization level, and if someone has a mix of exempt and non-exempt roles, MFA kicks in by default (better safe than sorry).
[Set MFA per organization](/authenticate/multi-factor-auth/mfa-per-org/)
</details>

<details>
<summary><strong>How do I help users who are having trouble with MFA?</strong></summary>

Make the instructions super clear for each method you support. For authenticator apps, walk them through the QR code scanning and emphasize saving those backup codes (they will lose them otherwise). For SMS, double-check they're entering phone numbers correctly with country codes. For email codes, check spam folder. Always give users a way to contact you when they get locked out - it happens to the best of us. 

You can also [reset MFA for a user](/manage-users/access-control/reset-multi-factor-authentication-for-a-user/).
[Multi-factor authentication guide](/authenticate/multi-factor-auth/enable-multi-factor-authentication/)
</details>

## Username authentication

<details>
<summary><strong>Do people still use username authentication? Does Kinde allow this?</strong></summary>

Absolutely Kinde supports this. Username auth is perfect when you want to give users more personality in their sign-in experience or when your app has that community vibe (think gaming platforms or developer tools). They'll still need to verify their email once for security (we're not animals), but after that they can sign in with their chosen username. Works with both password and passwordless methods, so you've got flexibility there.
[Username authentication guide](/authenticate/authentication-methods/username-authentication/)
</details>

<details>
<summary><strong>How does Kinde handle username uniqueness and case sensitivity?</strong></summary>

We treat usernames as case-insensitive because life's too short for "BuilderBob" vs "builderbob" authentication headaches. Users can still make their aesthetic choices like "RosyRose" or "DevDan" for display, but behind the scenes we're not picky about capitalization. This prevents fraud attempts and those frustrating "username not found" moments when someone forgets their exact capitalization.
[Username authentication details](/authenticate/authentication-methods/username-authentication/)
</details>

<details>
<summary><strong>What happens if a user changes their password when using both email and username authentication in Kinde?</strong></summary>

Both methods share the same password! If someone can sign in with both their email and username, changing the password for one affects both. It keeps things simple for users (one password to remember) and prevents the confusion of having different passwords for the same account. We think this makes way more sense than forcing people to juggle multiple credentials.
[Username authentication configuration](/authenticate/authentication-methods/username-authentication/)
</details>

## Device Authorization Flow

<details>
<summary><strong>When should I use Kinde's device authorization flow instead of regular authentication?</strong></summary>

Device authorization flow is perfect for situations where typing is a nightmare - think smart TVs, gaming consoles, IoT devices, or anything without a proper keyboard. Instead of watching users struggle with TV remote controls to spell out "MyComplexPassword123!", the device shows them a simple code to enter on their phone or laptop where typing doesn't suck. 

It's basically Netflix's approach: the TV shows a code, you enter it on your phone, boom - you're authenticated. Much better user experience, and way more secure. 
[Device Authorization Flow](/authenticate/device-authorization-flow/overview/).
</details>

<details>
<summary><strong>How does Kinde's device authorization flow work, and what should I tell my users?</strong></summary>

Here's the flow: your device (let's say a smart TV app) requests a device code from Kinde, then shows users a simple code and a URL like "Go to `yourapp.com/device` and enter code: ABC123". Users grab their phone, visit that URL, sign in normally (with all their usual auth methods available), enter the code, and authorize the device. 

Meanwhile, your TV app is polling our servers asking "Are they done yet? Are they done yet?" until we give it the green light with tokens. The whole thing happens in parallel - no hanging around waiting.
[Device Authorization Flow](/authenticate/device-authorization-flow/overview/)
</details>

<details>
<summary><strong>What are the security benefits of Kinde's device authorization flow?</strong></summary>

It keeps credentials off devices you don't control, which is huge for security. Users never enter their actual passwords on the TV, CLI, or IoT device - they authenticate on their trusted phone or laptop instead. Plus, they get access to all their usual security features like MFA, social sign-in, and password managers. The codes expire quickly, we rate-limit the polling to prevent abuse, and users always see a proper consent screen before authorizing access. It's basically taking the most secure part of OAuth (browser-based auth) and making it work for devices that can't do browsers. 
[About authentication methods](/authenticate/about-auth/authentication-methods/)
</details>

<details>
<summary><strong>How do I handle Kinde device authorization flow errors and edge cases?</strong></summary>

The main errors you'll see are `authorization_pending` (user hasn't finished yet - keep polling), `slow_down` (you're polling too aggressively - back off), and `expired_token` (codes expired - start over). Handle these gracefully in your app rather than crashing. Users might also get confused about which device they're supposed to use for what, so make your instructions crystal clear. And remember, some users will start the process but never finish it - that's normal, just clean up expired sessions.
[OAuth token validation and errors](/build/tokens/token-validation-errors/)
</details>

<details>
<summary><strong>What's the best UX for presenting Kinde device codes to users?</strong></summary>

Keep it simple and obvious. Show the code clearly (big, readable font), include the full URL they need to visit, and consider showing both a QR code and the manual entry option. Don't overwhelm them with too much text - just "Go to `yourapp.com/device` on your phone and enter: ABC123" works perfectly. If you can, show some kind of progress indicator so they know the app is waiting for them to complete the process. And please, test this with actual humans - what seems obvious to developers often isn't to regular users.
</details>

## Custom configurations & user experience

<details>
<summary><strong>What do I need to set up Kinde phone authentication for my users?</strong></summary>

You'll need a paid Twilio business account - this isn't something we can handle for free because SMS costs money. Before you dive in, check if you need 10DLC registration (10 Digit Long Code) for your region - it's required in some places for business messaging. Read up on Twilio's A2P (Application to Person) messaging guidelines too. Once you've got that sorted, plug your Twilio details into Kinde, pick between using their messaging service (better for global apps) or a specific phone number, and set your default country.
[Set up phone authentication](/authenticate/authentication-methods/phone-authentication/)
</details>

<details>
<summary><strong>Can I customize the SMS message that Kinde users receive?</strong></summary>

Nope, and here's why - we use a standard format that meets security best practices and works across different languages. The message looks like: "123456 is your one-time code to sign in to [xxxx@login.xxx.au](mailto:xxxx@login.xxx.au) #123456". That weird duplication at the end? It's for security. We know it might not match your brand perfectly, but it keeps everyone safe and compliant.
[Phone authentication details](/authenticate/authentication-methods/phone-authentication/)
</details>

<details>
<summary><strong>How can I create a more seamless Kinde sign-up experience for invited users?</strong></summary>

Use the `login_hint` parameter to pre-fill email fields when you know who's trying to sign in - it's like having their name already on the guest list. You can also create a unified experience where users don't have to choose between "sign up" or "sign in" (because honestly, who remembers if they've been here before?). Skip asking for first and last names if you want to keep things really minimal. Every little bit of friction you remove makes a difference.
[Pre-populate user identity](/authenticate/custom-configurations/prepopulate-identity-sign-in/) | [Manage authentication experience](/authenticate/custom-configurations/authentication-experience/)
</details>

<details>
<summary><strong>What's the best way to handle profile pictures and user data with Kinde?</strong></summary>

We automatically grab profile pictures from email providers like Google and use Gravatar as backup when pictures are missing. If you'd rather handle profile pics your own way or just hate blank avatars, you can switch off the Gravatar fallback. Fair warning: Apple is pretty stingy with user data - they don't pass through avatars or much profile info, so don't expect much there.
[Authentication experience customization](/authenticate/custom-configurations/authentication-experience/)
</details>

<details>
<summary><strong>How can I pass additional parameters to identity providers through Kinde?</strong></summary>

Upstream parameters let you send extra data during authentication - either the same value every time (static) or something unique per user (dynamic). Common use case: passing `login_hint` to pre-fill sign-in forms or enabling those handy account switchers you see on Google. Each provider supports different parameters (check their docs), and you can even rename parameters using aliases if your IdP is picky about naming.
[Pass parameters to identity providers](/authenticate/auth-guides/pass-params-idp/)
</details>

## Developer questions

<details>
<summary><strong>Why does Kinde authentication state get lost when users refresh the page in single-page apps?</strong></summary>

We store tokens in memory for security - it protects against both CSRF and XSS attacks, which is definitely worth the trade-off. But yeah, it means page refreshes wipe the tokens. The best fix? Use our Custom Domains feature, which lets us set secure httpOnly cookies on your domain. 

For local development, there's an escape hatch called `is_dangerously_use_local_storage`, but seriously, don't use that in production - the name isn't kidding about the danger part.
[JavaScript SDK guide](/developer-tools/sdks/frontend/javascript-sdk/) | [React SDK guide](/developer-tools/sdks/frontend/react-sdk/)
</details>

<details>
<summary><strong>How do I implement Kinde authentication in a React application without losing user state?</strong></summary>

Wrap your app in the KindeProvider - it's your new best friend for managing auth state. Use hooks like `useKindeAuth()` to check if someone's signed in, and always check the `isLoading` state before making decisions (nobody likes flickering UI). For production, definitely set up custom domains so you can use secure cookies. Handle your post-auth redirects properly, and your users will never know how complex this stuff really is under the hood.
[React SDK implementation](/developer-tools/sdks/frontend/react-sdk/)
</details>

<details>
<summary><strong>What's the best approach for handling Kinde authentication callbacks in different frameworks?</strong></summary>

Each framework has its own quirks. Next.js App Router wants `app/api/auth/[kindeAuth]/route.js`, Pages Router prefers `pages/api/auth/[...kindeAuth].js`, and vanilla JavaScript means you're handling the OAuth dance yourself. Always make sure your callback URLs match what you've configured in Kinde (case-sensitive, protocol-specific). Use our SDK callback handlers instead of rolling your own - we've already dealt with all the edge cases.
[Next.js App Router SDK](/developer-tools/sdks/backend/nextjs-sdk/) | [Using Kinde without SDK](/developer-tools/about/using-kinde-without-an-sdk/)
</details>

<details>
<summary><strong>How can I protect API endpoints and validate Kinde tokens properly?</strong></summary>

Use our backend SDKs or validate JWT tokens manually - either works, but the SDKs handle the fiddly bits for you. The `getToken()` method gives you bearer tokens for API calls. On your backend, always check the token's audience claim matches your API and verify it hasn't expired. And please, never put client secrets in frontend code - that's like leaving your house key under the doormat.
[TypeScript SDK guide](/developer-tools/sdks/backend/typescript-sdk/)
</details>

## Troubleshooting & common issues

<details>
<summary><strong>How do I use Kinde to help my users who forgot their passwords?</strong></summary>

Users can hit "forgot password" on the sign-in screen and we'll send them a one-time code via email to reset it. As an admin, you can also trigger password resets through the Kinde dashboard or API (as long as they have a verified email). There's also the option to set a temporary password for them, but you'll need to send it through your own channels - we won't email passwords directly because that's not secure.
[Password reset procedures](/authenticate/authentication-methods/password-authentication/)
</details>

<details>
<summary><strong>How should I help users who aren't receiving Kinde SMS auth codes?</strong></summary>

Start with the basics - did they enter their phone number correctly with the right country code? Do they have cell reception? Are they in a country where SMS might be restricted? Then check your end - is your Twilio account funded and configured properly? SMS delivery can be finicky, especially internationally, so having backup contact methods is always smart.
[Phone authentication setup](/authenticate/authentication-methods/phone-authentication/)
</details>

<details>
<summary><strong>How should I help users who aren't receiving Kinde auth codes via email?</strong></summary>

Start with the basics - could the email have been caught in their spam or been caught by their organization's firewall and have been added to a supression list? Have them check this first. If you have [custom SMTP email delivery](/integrate/third-party-tools/kinde-resend-custom-smtp/) set up, you should be able to check logs from the delivery provider. If you rely on Kinde to deliver emails, check the same basic things with the recipient and ask them to try again. If you need to, contact the Kinde support team to check our logs to see if there was an email disruption. 
[Phone authentication setup](/authenticate/authentication-methods/phone-authentication/)
</details>

<details>
<summary><strong>If I want to change which providers can be used for auth in Kinde, how do I support my customers?</strong></summary>

If users were relying on a social or enterprise connection that got removed or changed, they're stuck until you fix it. Before deleting any connection, make sure nobody's using it for auth. If you need to switch providers, set up the new one first, then help users transition by linking their accounts or setting up alternative auth methods. Always have a backup plan.
[Manage social connections](/authenticate/social-sign-in/add-social-sign-in/)
</details>

<details>
<summary><strong>Why are my Kinde authentication redirects failing?</strong></summary>

Nine times out of ten, it's a URL mismatch. Your callback URLs in Kinde need to match exactly what's in your app code and any social provider configs - we're talking case-sensitive, protocol-specific matching here. If you're using custom domains, double-check that your DNS records are set up correctly and SSL certificates are active. Also remember that custom domain tokens and Kinde subdomain tokens don't play nice together - pick one and stick with it.
[Custom domain setup](/build/domains/pointing-your-domain/)
</details>

<details>
<summary><strong>How do I debug Kinde OAuth 2.0 authentication errors?</strong></summary>

The error names are pretty self-explanatory once you know what to look for. `invalid_client` usually means wrong client ID or secret, `invalid_grant` means your authorization code expired (they only last a short time), and `invalid_scope` means you're asking for something we don't support. Check your credentials first, make sure you're exchanging codes quickly, and verify your requested scopes are valid. Give users helpful error messages instead of raw OAuth codes - nobody wants to see "invalid_grant" when they're just trying to sign in.
[OAuth 2.0 validation and errors](/build/tokens/token-validation-errors/)
</details>

<details>
<summary><strong>What should I check in Kinde when users report authentication isn't working on mobile?</strong></summary>

Mobile auth has its own special challenges. For React Native, make sure your deep linking is configured properly with the right URL schemes for both iOS and Android. Check that your redirect URLs use the correct custom scheme format like `myapp://your_kinde_domain.kinde.com/kinde_callback`. And here's a fun fact: Google doesn't support auth in webview, so make sure you're using proper browser-based flows. If your users are not receiving verification codes and you have Twilio set up, you can check the Twilio logs to help you troubleshoot.  
[React Native SDK](/developer-tools/sdks/native/react-native-sdk/) and [Set up phone auth with Twilio](/authenticate/authentication-methods/phone-authentication/)
</details>

<details>
<summary><strong>How do I handle Kinde authentication state persistence across different environments?</strong></summary>

For production, custom domains are your friend - they enable secure cookie storage that survives page refreshes. For local development, you can use the local storage escape hatch (just remember to remove it before going live). On your backend, implement proper session management using encrypted cookies or shared cache systems if you're running multiple servers. The right approach depends on your architecture, but security should always come first.
[TypeScript SDK session management](/developer-tools/sdks/backend/typescript-sdk/)
</details>

## Best practices & security

<details>
<summary><strong>What security considerations should I communicate to my customers about Kinde authentication choices?</strong></summary>

Be honest about the security trade-offs without scaring people away. Passwordless is genuinely more secure than passwords, MFA adds real protection (not just security theater), and social sign-in can be both convenient and secure when done right. If you're offering password auth, nudge users toward password managers - most people's password habits are... not great. And here's something most auth providers won't tell you: we store passwords as encrypted hashes that literally cannot be deciphered, so even we can't see what users set.
[Password authentication security](/authenticate/authentication-methods/password-authentication/)
</details>

<details>
<summary><strong>How do I ensure my Kinde authentication setup scales with business growth?</strong></summary>

Start simple and add complexity as you need it - don't over-engineer from day one. Begin with email auth, then layer in social sign-in, MFA, and enterprise connections as your customer base grows. Use organizations to handle multi-tenant setups where each customer needs their own user management. Set up your foundation at the environment level, then customize per organization when customers have specific needs. And seriously, implement custom domains early if you can - it makes everything smoother later.
[Kinde for different business models](/build/set-up-options/kinde-business-model/)
</details>

<details>
<summary><strong>What's the recommended approach for handling user migration from other auth providers to Kinde?</strong></summary>

Set up your auth methods in Kinde first, then export and import user data via CSV or JSON. If you import passwords too, your users won't notice anything changed - which is exactly what you want. If you're switching auth methods (like going passwordless), give users a heads up about what's changing. Test everything in a sandbox environment first, and keep an eye out for edge cases like users who change their passwords during the migration window.
[Switch to Kinde migration guide](/get-started/switch-to-kinde/switch-to-kinde-for-user-authentication/)
</details>

## Advanced integration

<details>
<summary><strong>How do I implement Kinde custom authentication pages while maintaining security?</strong></summary>

You can build your own sign-up and sign-in pages to match your brand perfectly, but we'll still handle the security-critical stuff like verification and MFA. Use connection IDs and login hints in your auth URLs to route users directly to specific authentication methods. Think of it as having your cake and eating it too - custom experience with bulletproof security. Just remember that some screens (password entry, code verification) stay with us because that's where the security magic happens.
[Custom authentication pages](/authenticate/custom-configurations/custom-authentication-pages/)
</details>

<details>
<summary><strong>What's the best way to handle cross-subdomain authentication in Kinde, for complex applications?</strong></summary>

Custom domains and proper cookie configuration are your best friends here. Set cookies to the root domain instead of subdomains so they're accessible across your entire ecosystem. For PHP apps, we've got helper functions to make this easy. Test everything thoroughly across all your subdomains - nothing's more embarrassing than users getting stuck switching between `app.yoursite.com` and `dashboard.yoursite.com`.
[PHP SDK domain configuration](/developer-tools/sdks/backend/php-sdk/)
</details>

<details>
<summary><strong>How should I configure Kinde authentication for different business models (B2C vs B2B)?</strong></summary>

B2C is straightforward - configure everything at the business level with easy social sign-in and email auth. B2B gets more interesting because you're serving multiple companies, each with their own needs. Use organizations to create separate tenant management, set up enterprise connections for business customers who need SAML or Entra ID, and keep simpler social auth for any consumer-facing parts of your platform. It's all about matching Kinde auth options to what your customer actually needs.
[Business model configuration](/build/set-up-options/kinde-business-model/)
</details>
