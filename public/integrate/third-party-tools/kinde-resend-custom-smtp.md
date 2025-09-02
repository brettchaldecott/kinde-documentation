
You can configure Kinde to send emails using your existing Resend account. This means your users will receive communications from your business’s email address, and not Kinde’s.

Other benefits include better control of email deliverability, analytics and tracking, scalability, email compliance, etc.

Please review the documentation at [Customize email sender](/get-started/connect/customize-email-sender/) for details about how custom SMTP works with Kinde and third party providers. 

## Generate an API key from Resend

You'll need an API key from Resend to use as the password for authenticating with their SMTP server.

You can use an existing API key from Resend if you have one already.

Or you can create a new API key from the Resend dashboard by going to the API keys section and clicking `Create API Key`. 

See Resend's [API Keys](https://resend.com/docs/dashboard/api-keys/introduction) documentation for more details. 

## Whitelist domain

Resend also requires you to verify the domain being used for the emails. See Resend's [Managing Domains](https://resend.com/docs/dashboard/domains/introduction) documentation for verifying your domain.

## Add email sender SMTP details

If you are setting this up and your production environment is already live, we recommend testing this in a staging or test environment first.

1. In Kinde, go to **Settings > Email**.
2. Enter a **Sender name**. This is usually your business name, but might be a specific person.
3. Enable the **Use custom sender** switch. The SMTP details section opens.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/d4c4b57d-9f1a-4b1c-638c-0a770c4fe400/public"
  alt=""
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

4. Enter the **Sender email**. This is the address your users will receive emails from and should be one of the verified domains from your Resend account.
5. Enter the SMTP details of your email provider:
   - Server name - `smtp.resend.com`
   - Port - `465`
   - User - `resend`
   - Password - Use the API key generated from your Resend account
6. Select **Send test email**. This sends a test email to the email address you are logged into Kinde with.
7. If the test is successful, select **Save**.
8. If the test is not successful, check the SMTP details (step 5) and try sending a test email again. If this does not work, see the troubleshooting section below.

## Troubleshooting

If sending a test email fails:
- Verify network connectivity to smtp.resend.com.
- Confirm the port, user, and API key are correct.
- Review error messages on the Resend dashboard.

For more details, see Resend's troubleshooting guide: https://resend.com/docs/dashboard/api-keys/troubleshooting
