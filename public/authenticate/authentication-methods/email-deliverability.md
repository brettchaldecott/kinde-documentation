
Kinde encourages users to configure their own email sender, so that verification and other emails are sent form your own business, and not Kinde.

When emails are sent via Kinde, we support the email routing, and are proactive about monitoring verification email deliverability and speed.

This topic describes email delivery via Kinde, deliverability factors, and some common reasons why delivery can fail.

## Manage your own email sender

By default, when you first start using Kinde, all emails are sent from @kinde.com. But you can set up your environments to use your own email, so users receive authentication emails from @yourbusiness.com.

You’ll want to configure this before your production environment goes live. All you need is to add SMTP details in Kinde settings. See [Customize email sender](/get-started/connect/customize-email-sender/).

## Emails sent from Kinde

Even if you enter custom sender details, these emails will still be sent from Kinde:

- Invitation to join business team - triggered by manual or API addition of team member
- Warnings and notifications of data export

## Deliverability factors

Email delivery is dependent on a number of factors.

### IP address reputation and blocklists

Email providers check against a pool of IP addresses and domain blocklists to help protect against bad actors. They constantly monitor to make sure IP addresses are not on any of these lists. If you experience issues, check that your domain doesn't exist on any of these lists.

### Domain name reputation

Every domain name (i.e. `example.com`, `kinde.com`, etc.) has its own reputation score. Newer domains do not have a high score, and this may impact deliverability.

### Setup a real email address

Email providers will check if there's an actual mailbox behind the "from address" of an email. Make sure when you set your custom email sender, that you use a real email address such as `notifications@yourbusiness.com`.

### Email content

Kinde email content is optimized to (as far as we can) ensure our communications don’t get identified by providers as spam. Maintaining deliverability is one of the reasons we have limited the ability to edit email content.

### SPF, DMARC, and DKIM for email authentication

SPF (Sender Policy Framework), DMARC (Domain-based Message Authentication, Reporting, and Conformance), and DKIM (DomainKeys Identified Mail) are email authentication methods used to combat email spoofing, phishing, and other forms of email fraud.

These records add a digital signature to every outgoing message, which allows your provider to verify that emails were actually sent from you. Almost all email providers look for these to be set as a strong signal of legitimacy.

### Strengthen email authentication with your provider

You and your email provider are ultimately responsible for ensuring the right level security and risk management for email authentication. Because Kinde allows you to use any provider you like, check their documentation to find out if their policies and approach meet your needs.

## Provider-related issues

Despite all we can do, verification emails still occasionally end up in spam or quarantined. The cases below are specific, but might help you troubleshoot is they arise.

### Gmail

Delivery addresses that are part of Google Workspace can sometimes be delayed by about 4 minutes, due to pre-delivery message scanning. It can help to sign up for Gmail's postmaster tools, to help troubleshoot issues.

### Microsoft (Hotmail / Outlook / Office365)

Microsoft Defender's aggressive anti-spam filters sometimes stop verification emails reaching certain Outlook inboxes. Then the email is placed in quarantine and the administrator has to restore it, for it to be delivered. With Kinde, this should be rare, as we only send OTPs and not magic links. Access Outlook Sender Support and check you are following recommendations.
