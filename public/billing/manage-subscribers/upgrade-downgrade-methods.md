
Customers will eventually want to change the plan they are on and you need a way to alter their subscription.

## How to upgrade or downgrade a plan subscription

- Set up the self-serve portal and allow customers to manage their own subscriptions. See [self-serve portal for orgs](/build/set-up-options/self-serve-portal-for-orgs/) and [self-serve portal for users](/build/set-up-options/self-serve-portal-for-users/). In this scenario, the user will select the new plan from the pricing table.
- Manually upgrade or downgrade via the Kinde UI (see below)
- Overwrite the current agreement (subscription) by [creating a new one via API](/kinde-apis/management/#tag/billing-agreements/post/api/v1/billing/agreements/)

## Change a plan subscription for an organization (B2B) or user (B2C)

1. Open the organization or user record in Kinde.
   - Go to **Organizations** and locate the org.
   - Go to **Users** and locate the user. 
2. Select **Billing** in the side menu. Details of the org's or user's plan are shown.
3. Select **Change plan**.
4. In the dialog that appears, select the new plan.
5. Select **Save**. The customer's plan is now updated.

Depending on the [billing policies](/billing/manage-plans/upgrade-downgrade-plans/) you have set, this information will be synced to Stripe and an invoice credit or bill will be issued.
