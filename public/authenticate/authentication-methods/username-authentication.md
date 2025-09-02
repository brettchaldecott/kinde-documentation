
Along with email and phone number, Kinde supports authentication where a username is the user’s sign-in identity.

On sign-up or registration, the user will need to do a one-time validation of their identity via email - for security - but they can subsequently use a username to sign in.

<Aside title="Capture other names with properties">

The `username` field is designed for use in the authentication flow, if you want your customers to set a username, display name, or handle as part of their profile in your app, you can add a different property to capture this data. See [Add and manage properties](/properties/work-with-properties/manage-properties/).

</Aside>

## Usernames must be unique

There are several ways usernames can be added to a user’s profile:

- Manually in Kinde
- Via API
- Self-created by the user on registration
- imported with user profiles

Kinde treats usernames as case-insensitive. In other words, we ignore case. We do this because it eliminates the possibility of auth issues and fraud when two usernames are identical in every aspect except the case of one of their letters. 

We are happy to support users choosing an aesthetically pleasing username combination, like `RosyRose` or `BuilderBob`. We just don't also support separate identities for `rosYrosE` and `BUilderbob`. 

Regardless of how a username is added, it must be unique (in more than case). If a username already exists, an error will be returned.

## The sign-up flow

For security reasons, Kinde doesn’t allow fully anonymous users. So when a user signs up, they will need to supply an email, in addition to a username. The email can then be used to verify their identity. The username can be supplied by you, or can be created by the user.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/c4f7ed33-ef8b-442e-14f0-bc12a4f5c100/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

## The sign-in flow

When a user signs in, they enter their username and proceed with a [password](/authenticate/authentication-methods/password-authentication/) or a [passwordless OTP](/authenticate/authentication-methods/passwordless-authentication/).

Either way, it’s a quick process for sign in.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/ce25d6b7-0e5c-4f0c-0851-996a8315fb00/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

## Why an email is required

In order to be sure that you are signing up a real person, you need to have a way of contacting new users to verify their identity. Without identity verification, the authentication experience you provide could be vulnerable to security threats, fraud, bots, etc.

Once an email is verified, we add this email identity for the user. If the auth method is passwordless, this is where we send the OTPs. An email is also required for [password resets](/manage-users/access-control/reset-user-password/).

## Enable username authentication in Kinde

1. In Kinde, go to **Settings > Authentication**.
2. Select **Configure** on the **Username** (password) or the **username + code** (passwordless) tile. A configuration window opens.
3. Select which apps will support username authentication.
4. Select **Save**. The sign up flow will be updated for the applications you selected.

## Rules for usernames

- Usernames must be unique
- 2-64 characters, no spaces
- Can include letters, numbers, -dashes, \_underscores (no special characters)
- Case is ignored. Jane and jane are treated the same.

## One password for multiple identities

Users can only have a single password in Kinde.

If you allow both email-password and username-password authentication for a user, the password is shared across both their identities. For example, changing a user’s password for username affects their email sign-in and vice-versa.

See [the password rules](/authenticate/authentication-methods/password-authentication/#password-strength).
