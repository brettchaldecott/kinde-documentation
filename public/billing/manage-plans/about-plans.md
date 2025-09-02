
Plans enable you to structure your app features and charge your customers for using your services. Kinde supports both basic and more advanced plans for SaaS services. Here’s some examples:

- A simple subscription plan: $10 per month for x features
- A more complex plan: $20 per month for x features, plus x amount per GB storage, plus MAU charges (up to x free per month).

We recommend creating your basic or simplest plan first (e.g. your Free plan), then work through to more complex plans.

## Current limitations

These are known limitations that we are actively working on to add.

- Support for plan models with free trial periods.
- Annual subscriptions. Only monthly is available right now.
- No alterations to billing cycle or invoice methods.
- Add-ons and discounts that can be applied to individual subscriptions.

## About plan groups

Plan groups are a collection of related plans, tied to either users or organizations. 

- You can create as many plan groups as you need. 
- Each group can only contain plans for users OR plans for orgs.
- You can create multiple pricing tables for one plan group (but only one is the default). 
- Only one plan group can be included in a pricing table.

For more information see [add and manage plan groups](/billing/manage-plans/add-manage-plan-groups/)

## Parts of a plan

There are a number of concepts and elements involved in building a plan.

### Plan profile

This is the name of the plan, description, code key, etc. You need to enter these details and save them before you move on to adding charges and features.

The plan name appears on the pricing table that you can generate to share with customers. We recommend keeping it short. Use the description fields to explain differences in plans for internal use.

### Plan feature pricing

Depending how you manage plans and feature provisioning in your application, plan pricing might be simple or complex in your business.

When you set up a plan, you can include a mixture of any of these options:

- **Fixed charges** - a recurring charge that does not change and is invoiced in advance, e.g. a monthly subscription charge.
- **Metered features** - usage-based entitlements that define what and how much users’ access, and what they pay to access it. These might be per unit, per tier, or non-chargeable. Such as included and additional MAU.
- **Unmetered features** - things included in a plan that users don’t pay for separately. Unmetered features are generally used to gate feature access. E.g. X feature is not available on the Free plan, but is on a higher tier plan.

## Example plan features

| Feature             | Price                        | Pricing model             |
| ------------------- | ---------------------------- | ------------------------- |
| Access to x feature | N/A                          | Unmetered                 |
| Subscription        | $ 20 / month for access      | Fixed charge              |
| Interaction         | $x per interaction           | Metered / tiered          |
| Storage/Usage       | $x per GB                    | Metered / units or tiered |
| MAU                 | x included                   | Metered / non-chargeable  |
| MAU                 | Additional $x per MAU over n | Metered / tiered          |

A plan is complete when it includes all the features needed to provide the access and charge the right price.

## How to use and re-use features and charges

Kinde structures plans so that they can share a pool of features and charges that you define at a high level. When you add the feature in a plan, you add the price, limits, and other differences uniquely in each plan. This makes building and managing plan features easier, because the feature shares the same key (e.g. `base_price`), making it easier to manage in your code and for gating.

For example, you only need to define the item ‘Base price’ once, and then you define the price when you add it to each individual plan.

Similarly, features that are inclusions, such as support hours included, just create one metered feature. Then set the limits of the feature differently for each plan level. E.g. Free plan one hour, Pro plan gets 2 hours, etc. 

![Model of how you can use the same feature or charge across plans](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/190e7ba8-ba26-4fb6-57b4-a4d6b9fec700/public)

## Draft and published plans, and plan versioning

Plans can be in two states:

**Draft** - these plans are being developed and are not available for subscribers yet.

**Published** - products and prices are synced to Stripe and the plan can be signed up to manually, or via API.

- Once a plan has a subscriber, the plan cannot be changed, only versioned.
- Published plans must be included in a pricing table for self-sign ups.

Plan versioning is being worked on, but is not currently available.

## When a plan feature price syncs to Stripe

Feature prices sync to Stripe when you are successfully connected to Strip and you publish the plan containing that feature. When you are creating a plan, you may notice that some features show `price synced` or `price not synced`.

- Price synced - The price in Kinde and Stripe are the same. A plan with this feature is published.
- Price not synced - The price in Kinde has not yet synced to Stripe. When you publish or re-publish the plan, this will change.

If you see a mix of synced and not synced feature prices on a plan, this might be because the synced ones are in a plan that is published.
Unmetered (non-chargeable) features do not have a price sync status badge.
