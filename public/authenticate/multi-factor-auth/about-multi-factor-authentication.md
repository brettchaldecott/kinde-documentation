
To increase security for your product, you can enable multi-factor authentication (MFA). This means that your users sign in using at least two authentication methods, for example, password _plus_ verification code.

If you don't want to apply MFA settings for all users, you can set [MFA per organization](/authenticate/multi-factor-auth/mfa-per-org/) if you're on the Kinde Scale plan.

## **Available MFA factors**

Kinde supports the following secondary factors for MFA.

- **Email** - users are sent a one-time-password (OTP) via email
- **SMS** - users receive a one-time-password (OTP) via SMS
- **Authenticator app** - users receive a verification code via an authentication app. You might recommend a specific app or allow users to choose.

We recommend against choosing a secondary factor that is the same as the primary auth method. For example, if the primary method is email/passwordless, then choose SMS or Authenticator app as the secondary factor.

## **The MFA experience for users**

If you make MFA optional, users will be prompted to opt in to MFA when they next sign in.

If mandatory or after they opt in, users will be prompted to use (or choose) a secondary authentication method, through which they will receive a verification code. They will also be offered a set of recovery codes (See below).

We suggest you advise users ahead of time if you are changing your sign-in requirements, and if you require them to download an authenticator app such as Google Authenticator.

### MFA **using an authenticator app**

- On first time use, the user can scan a QR code to enable the verification method and get a verification code sent to their app of choice.
- On subsequent sign in, a verification code will appear in their authenticator app of choice, or they can use a recovery code to sign in.

### MFA **using email verification**

- A code is sent to their email that they need to enter into the verification code field on the sign up / sign in screen.

### MFA **using SMS verification**

- A code is sent via SMS that the user must enter into the verification code field on the sign up / sign in screen.

## **Recovery codes**

When a user signs in for the first time, or signs up as a new user (and MFA is activated), they will be offered a set of recovery codes that they can store for future use. They can then use a recovery code if they don’t have access to their device or authenticator app.

## MFA can be enforced for individual organizations

Customers on the Kinde Scale plan are able to set MFA at the organization level. This is especially useful for B2B businesses who have many organizations, with varying auth requirements. See [Set MFA for an organization](/authenticate/multi-factor-auth/mfa-per-org/).
