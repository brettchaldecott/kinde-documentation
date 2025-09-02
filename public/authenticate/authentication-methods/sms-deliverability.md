
Kinde encourages customers to configure their own SMS sender, so that verification, country registrations, sender IDs, and deliverability metrics are controlled and managed by you.

When SMS messages are sent via Kinde, we use our regional shared service provider and attempt delivery on a best-effort basis. As Kinde is a service provider, there are limitations when applying for dedicated short codes or sender IDs in countries with strict SMS-sending regulations.

This topic describes SMS delivery via Kinde, deliverability factors, and some common reasons why delivery can fail.

## Manage your own SMS sender

By default, when you first start using Kinde, all SMS messages are sent from Kinde's shared service provider. We recommend configuring your own SMS sender so that users receive authentication messages branded with your business.

Configure this before your production environment goes live. Simply add your SMS details in Kinde settings. See [Set up phone or SMS authentication](/authenticate/authentication-methods/phone-authentication/) for step-by-step instructions.

## Deliverability factors when using Kinde's default SMS shared service provider

Kinde provides no guarantees on SMS delivery and attempts delivery as best effort only.

### Countries with known good delivery due to Kinde involvement

The following countries have been tested and are known to have good delivery due to Kinde registering with the respective regulatory bodies.

- Australia (LONG CODE, SENDER ID)
- Canada (LONG CODE)
- Great Britain (SENDER ID)
- Ireland (SENDER ID)
- United States (TOLL FREE)

### Countries with known good delivery without Kinde involvement

The following countries have known good delivery without Kinde registering with the respective regulatory bodies.

- Generally the rest of the EU
- South Africa

### Countries with known bad delivery

The following countries have known bad delivery rates.

- India
- New Zealand
- United Arab Emirates

## Sender ID

Kinde has set up a shared Sender ID for all customers using the default SMS shared service provider. This cannot be changed.

**Sender ID:** `KindeAuth`

To brand the Sender ID to your business, you will need to configure your own SMS delivery provider. See [Set up phone or SMS authentication](/authenticate/authentication-methods/phone-authentication/).

## SMS content

The content of the SMS is not editable due to strict one-time passcode requirements and industry best practices.

An example of the SMS content is below.

```
123456 is your one-time code to sign in to {Business Name} 

@business.kinde.com #123456
```
