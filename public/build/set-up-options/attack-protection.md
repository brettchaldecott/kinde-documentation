
Attack protection is always on, and Kinde ships with sensible defaults to keep your product protected without you lifting a finger. There are some things you can configure.

## Set brute force protection

1. In Kinde, go to **Settings > Attack protection**.
2. Select **Brute force protection**.
3. Set how many sign-in attempts users get before being locked out of their account. You can choose the Kinde default of 5 or set a custom amount.
4. Set how long the account lockout lasts before users can sign in again. You can accept the Kinde default of 5 minutes or set a custom time, up to 60 minutes.
5. Select **Save**.

### What counts as a failed sign-in attempt

- incorrect password entered
- incorrect OTP code entered
- incorrect recovery code entered
- incorrect MFA response entered

## Enable credential enumeration protection

Enumeration attacks are where an attacker tries to verify if an account exists using your credentials. One of the ways an attacker knows you have an account or not, is if they enter credenitals (e.g. email or phone number) and the screen either progresses to a password/code entry screen, or shows a message that the account does not exist. 

Once an attacker knows an account exists, they can go about breaking in. To prevent them ever knowing, you can ensure that the sign in experience does not give the answer away.

1. In Kinde, go to **Settings > Attack protection**.
2. Select **Enumeration protection**.
3. Switch on the toggle for **Credential enumeration protection**.
4. Select **Save**.

For general information about Kinde security, practices, and policies, see the [Trust Center](/trust-center/security/security-at-kinde/).
