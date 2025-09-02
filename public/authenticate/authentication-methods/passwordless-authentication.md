
Passwordless authentication is a type of authentication that does not require end-users to set or maintain passwords for access to an application. Instead, they authenticate using a one-time passcode (OTP).

<Aside>

The email + passwordless method is switched on by default in all new Kinde businesses

</Aside>

## About one-time passcodes (OTPs)

Kinde does not support magic links as a password alternative, instead, we prefer to use one-time passcodes (OTPs) as they are more secure, and require manual entry as opposed to a single click.

For example, someone with access to your email could click a link to get instant access to an application, but they cannot use the code unless they have initiated the correct sign in flow and have your sign-in identity as well. If you receive the OTP via SMS, someone would need to have your device and unlock code, to access it.

A OTP can be issued via email or phone, depending how you have set up authentication. It is also common to use OTPs as a factor in [multi-factor authentication](/authenticate/multi-factor-auth/about-multi-factor-authentication/).

Passcodes issued from Kinde expire after 2 hours.

## Set up passwordless authentication

1. In Kinde, go to **Settings >** **Authentication**.
2. In the **Passwordless** section, select **Configure** on the relevant tile.
3. If you select the **Email + code** tile:
   1. Select which applications will use this authentication method.
   2. Select **Save**.
4. If you select the **Phone** tile:

   1. Select which applications will use this authentication method.
   2. Select **Save**.

5. If you select the **Username + code** tile:

   1. Select which applications will use this authentication method.
   2. Select **Save**.

   <Aside>

   **You can test this feature** but passwordless phone authentication requires that you have a [Twilio](https://www.twilio.com/en-us) account. You need to enter your Twilio account details and [upgrade to Kinde Pro](https://kinde.com/pricing/) if you want your users to authenticate this way. [Learn more](/authenticate/authentication-methods/phone-authentication/).

   </Aside>

## If a user does not receive a code

It should not happen often, but occasionally users do not receive their passcode. Here's a few suggestions.

- Tell the user to check their junk folder - some email providers, systems, and devices have security in place to prevent spam. An OTP from an unknown provider (like Kinde) might get accidentally treated as such.
- Once or twice we have come across a domain provider who has added Kinde to a denylist and OTPs from us get rejected. You'll need to contact us so we can investigate and arrange allowlisting. This is a very rare cause of failed OTPs.

## Attack protection settings

Kinde allows you to control the number of sign-in attempts a user gets, how long they get locked out after a failed sign-in attempt, etc. See [Attack protection](/build/set-up-options/attack-protection/).
