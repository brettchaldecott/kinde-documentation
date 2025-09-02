
If emails are part of your authentication processes for users - for example, users receive sign-in codes via email in a passwordless process - you can customize email details, with some limitations.

## **Change the email sender** **name**

You can add your business or brand name so it appears as the sender of emails.

1. Go to **Settings > Email**.
2. Enter your business or brand name in the **Sender** name field.
3. Select **Save**.

## Change the email sender address

If you want to change the email sender address, you add your own email provider’s SMTP details. See [Customize email sender](/get-started/connect/customize-email-sender/).

## **Change email logo**

Emails inherit the logo you set as part of your [global brand](/design/brand/global-brand-defaults/). But you can also set a unique logo [for each organization](/design/brand/apply-branding-for-an-organization/) in your business.

## Change who verification emails are sent from

_Only applies if you choose not to send emails from your own provider_

When Kinde sends an email on your behalf, it is sent from the ‘Business name’ listed in your Kinde business by default.

For example:

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/aad2b06f-1a1d-4da0-8643-ef3709cb9c00/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

If you want emails to come from your app name instead (if it is different), then you can edit the business name in **Settings > Business > Details**, without it affecting your Kinde domain or other environment settings.

## Change the content of the one-time passcode (OTP) email (passwordless auth)

You can change the content of the OTP email if you use a [custom email sender](/get-started/connect/customize-email-sender/) for sending OTP emails. Once this is set up, you can make content changes as follows.

1. In Kinde, go to **Design > Emails > Email content.**
2. Select the language you want to edit. Only [languages you have selected](/design/content-customization/set-language-for-pages/) in your business are available.
3. You can edit the subject line, body content, and disclaimer footer. Use the placeholders to personalize and define where the code appears.
4. When you have made your changes, select **Save**. The new email content will now be sent.

## Add sign-in code to the email subject line (passwordless auth)

You can include the sign-in code in the subject line of OTP emails to make it visible for users in notifications. Note that it is less secure to do this, as the code can be seen by anyone when notifications are displayed openly on devices.

1. In Kinde, go to **Design > Emails > Email content.**
2. Select the language you want to edit. Only [languages you have selected](/design/content-customization/set-language-for-pages/) in your business are available.
3. In the subject line field, include the `${code}` placeholder. This is where the code will appear. E.g. Enter `${code}` for access.
4. Select **Save**.
