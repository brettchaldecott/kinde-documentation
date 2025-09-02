
In the Kinde model, we handle everything except the payment processing part of billing. Kinde integrates with a third-party payment processor (Stripe) for secure payment processing.

This involves a continuous sync between Kinde and Stripe, to ensure that products, prices, subscription information, invoices and payments, are accurate in both systems.

Kinde does not store payment details, such as credit card information. This is exclusively managed by Stripe.

## Billing for B2B, B2C and B2B2C

Kinde supports billing models for B2B, B2C and even B2B2C. Depending which you are setting up, you may need to do a few different tasks. Most of the setup is common, but we will call out tasks that are only relevant to one or the other.

- B2B - customers are companies, organizations, or groups.
- B2C - customers are individual users.
- B2B2C - the platform model, where you have a customer who is an organization, and then users who are customers of that organization.

## Kinde and Stripe

Kinde integrates a single payment processor (only Stripe Billing for now) to handle the financial and payment management side of things.

Stripe uses the plan data and the customer info to create an agreement in Stripe. The customer is invoiced based on this agreement. Stripe securely stores your customer’s payment details and Kinde never sees credit card or other bank information.

Here’s what the billing feature looks like as a workflow.

![Plan purchase workflow](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/b9a7bc9a-8f32-4a6d-fe4c-799db02c1600/public)

Stripe is currently the only payment provider supported with Kinde. But we plan to expand to other providers in the near future.

## Multi-currency support

Kinde has customers everywhere and almost every global currency is supported.

## Transaction data and Stripe region

- When you connect your Stripe account to Kinde, you connect to our Stripe US account. We do this because Stripe US more widely supports global functionality.
- Regardless of your own Stripe account region, you can still select any currency for your plans in Kinde, and Stripe will do the hard part of exchange rate conversion, tax calculations, etc.
- Any fees you incur in Stripe for international transactions are your sole responsibility.

## Billing and invoice cycles

By default, Kinde uses the following billing behavior, though some aspects can be customized depending on how you configure cancellations.

- Billing cycles are monthly, based on the customer's original sign-up date.
- Fixed charges (e.g. monthly subscription fees) are billed in advance, customers pay at the start of their billing period.
- Metered (usage-based) charges are billed in arrears, usage is tracked throughout the billing period, and charges appear on the next invoice.

You can also [set policies](/billing/get-started/setup-overview/) to control what happens when a customer cancels or changes their plan.
