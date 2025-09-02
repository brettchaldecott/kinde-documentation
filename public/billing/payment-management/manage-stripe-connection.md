
When you connect to Stripe as part of Kinde's billing feature, a new Stripe account is created for you. You cannot connect an existing Stripe account.

When you first connect Stripe as part of the setup flow, you need to set up the new Stripe account with all your business and other details to make the connection active. To connect, follow [this process](/billing/get-started/connect-to-stripe/). You can then manage the connection in **Settings > Billing**.

![Stripe connection card in Kinde](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/859d5538-db3c-4244-3dbe-7a2ff2c04b00/public)

## Manage the Stripe connection

The Stripe connection status (see below) lets you know if Stripe is connected or if you need to take action. Statuses include:

- `Connecting` means Stripe is still sending information to Kinde about the account status. It should not take long to sync.
- `In progress` indicates that Stripe has connected to Kinde, but still requires some additional details from you in order to fully set up the account. Select **Update Stripe information** to complete the Stripe onboarding. You need to do this before plans can be made available to your customers.
- `Connected` means Stripe is successfully syncing data with Kinde and you’re ready to publish plans.

You may also see an error and message, letting you know what action is needed.

To add and edit Stripe business information, select **Update Stripe information**.

## Disconnect Stripe

This action is not reversible.

1. Go to **Settings > Environment > Billing**.
2. On the Stripe connection card, select the three dots, then select **Disconnect**.
3. In the confirmation window, select **Disconnect billing connection**.

## Troubleshoot Stripe issues

Stripe requires that your account is set up properly before connecting successfully. Here's some common reasons why the Stripe connection status remains **In progress**.

- Incomplete business information - contact, tax, or other business details
- Identity verification - where you need to upload a copy of your ID

To add details in Stripe, select the **Update Stripe information** option.
