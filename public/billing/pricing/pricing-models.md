
A pricing model refers to the structured approach businesses use to charge customers for products and services, often based on factors like usage, feature access, or licensing. In Kinde, each feature you include in a plan has a pricing model attached. This topic describes some examples of pricing models you can apply in Kinde, when you might want to use them, and how to [set them up in your Kinde plans](/billing/manage-plans/create-plans/).

## Pricing decisions 

There's a lot of information available about SaaS pricing strategy, and many things to consider. Research the market and map out an approach that works for your business now and as you grow, before setting up plans for the first time. We will also add more flexibility and variation for pricing models in Kinde, as our billing feature develops.

## Flat rate pricing (e.g. subscription pricing)

Where you want to charge a periodic (e.g. monthly) flat fee for access to your product or services. Higher plans may include access to more features, or have higher access limits. 

In Kinde, create a **Fixed charge** and call it something like `Subscription fee`. Re-use this charge in each plan, and change the price and line item description.

- Example - Basic plan 5.00, Team plan 20.00, Business plan 75.00.

## Usage-based (metered or tiered) pricing

Where you charge customers for what they use. For example, monthly active users, data storage, transactions, etc. For each individual plan you can treat this as fully pay-as-you-go, or charge volume prices so the price goes down the more units are used. You can set limits for usage based pricing or leave as limitless. Limits can make plan upgrade more attractive. 

<Aside>

If you need to, you can [add metered amounts to individual customer plans via the Kinde API](/billing/manage-subscribers/add-metered-usage/). 

</Aside>

### Usage pricing - per unit

In Kinde, add a **New metered feature**, e.g. `Data storage`. Leave **Maximum units** blank for limitless, or set a limit. Select a **per unit** pricing model. Set a price for each unit. E.g.

- Example - Basic plan is 00.30 per unit, Team plan is 00.20 per unit, Business plan is 00.10 per unit.

### Usage pricing - volume pricing

In Kinde, add a **New metered feature**, e.g. `Token generation`. Leave **Maximum units** blank for limitless (you might do this for the highest plan only), or set a limit per plan. Select the **Tiered graduated** pricing model. Set the price for each unit tier for each plan. E.g. 

- Basic plan (100 unit limit) 0-10 tokens / 0.50 per token, 11-20 tokens / 0.35 per token, 21-100 tokens / 0.20 per token.
- Team plan (250 unit limit) 0-10 tokens / 0.40 per token, 11-20 tokens / 0.30 per token, 21-100 tokens / 0.15 per token.
- Business plan (No unit limit) 0-100 tokens / 0.15 per token, 101-250 tokens / 0.10 per token, 251-1,000,000,000 tokens / 0.05 per token. (Note you need to include an upper tier limit even if you offer limitless, just use a really high number.)

## Per feature pricing

Where you charge per feature or module in your SaaS model. This approach can make plan pricing transparent and make it easy for customers to choose the right plan. It can also help you group "expensive" or more "enterprise" features for inclusion in higher plans. However, it can also be challenging to decide what features should be in or out of specific plans, and to provision and gate each feature in-product.

If you want to charge based on features, we recommend using a subscription model, and then including/excluding features for your plan levels. 

- Example - Basic plan 10.00 for features A-E. Team plan 20.00 for features A-N. Business plan 75.00 for features A-Z.

Another way to approach this is to included limited metered features per plan, so that all customers get a "taste" of features, but to get more of one you need to upgrade.

- Example - Basic plan 10.00 for features A-E (plus one each of F & G). Team plan 20.00 for features A-N (Plus two of U & V). Business plan 75.00 for features A-Z.

## Support and feedback

Email support@kinde.com if you need a different pricing model than those listed above, or if you need help setting up your plan pricing.
