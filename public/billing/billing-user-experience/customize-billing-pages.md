
There are three ways you can allow customers to sign up to a plan.

## Option 1: Use your own plan selection screen

Display a plan selection screen in your own app or website. Once the user selects a plan, redirect them to Kinde to complete the payment flow.
To pre-select a plan, pass the `plan_interest` query parameter in the Kinde authentication URL.

## Option 2: Use Kinde’s built-in plan selection

Let Kinde handle plan selection as part of the authentication flow. To enable this:

- [Create a pricing table](/billing/billing-user-experience/plan-selection/) in Kinde
- [Add and publish plans](/billing/manage-plans/create-plans/)
- Set the table to **Live**

Once this is done, use the relevant pricing table key in the authentcation URL and Kinde will display the plan selection screen during signup.

## Option 3: Assign a plan via the Kinde Management API

Use the [Kinde Management API](https://docs.kinde.com/kinde-apis/management/#tag/billing-agreements/get/api/v1/billing/agreements) to assign a customer to a specific plan.
The next time the customer signs in, Kinde will automatically prompt them for payment details.

This is ideal for:

- Migrating customers from another system
- Assigning a plan without user input

## Customize billing screens in the authentication flow

Depending on how you choose to onboard customers, users will see three different billing screens during the authentication flow. These screens can be customized:

1. **Plan selection** - Displayed when a user signs up for the first time and multiple plans are available.

2. **Payment details** - Shown after a user selects a plan—or if a plan was pre-selected before redirecting to Kinde.

3. **Success** - Shown when sign up and payment are successfully completed.

By default, all screens will inherit branding and styling from the global Kinde [Design](/design/brand/global-brand-defaults/) settings.

If you want more control, you can [fully customize these screens using your own **HTML, CSS, and JavaScript**](/design/customize-with-code/customize-with-css-html/).
