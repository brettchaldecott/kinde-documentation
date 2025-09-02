
'Billing' refers to the broad function of creating plans, setting pricing, collecting payments, etc. Here are some of the common concepts and terms you will come across.

## Billing identifiers

- **Customer ID** - A `customer_id` uniquely identifies a user or org who has signed up to a plan. This is different to a `user_id` or `org_id` that identifies the user or org as an entity. 
- **Customer Agreement ID** - A `customer_agreement_id` identifies the contract or agreement created in Stripe when a plan is signed up to. The agreement ID is then associated with the `customer_id` in Kinde.

## Key concepts

- **Plan groups** - Plan groups organize multiple plans under a common category, helping segment offerings by use case, customer size, or market. A group is scoped to either organizations (B2B) or individual customers (B2C). You can have multiple plan groups, but each plan must belong to a group.
- **Plans** - Plans define the specific features, usage limits, and pricing tiers offered to customers in a SaaS product.
- **Features** - Plan features are the specific capabilities or entitlements included in a plan. They can be chargeable (incurring additional fees) or non-chargeable (included at no extra cost). Features can be metered (usage-based) or unmetered (fixed access).
- **Fixed charges**- Single chargeable items like the subscription base price and other stand alone recurring charges.
- **Pricing models** - Pricing models are the different methods of charging customers, such as flat-rate, usage-based, tiered, per-seat pricing, etc. Your pricing model is determined by your product and customer needs. Consider scalability and longevity when deciding this.
- **Payments processor** - A payments processor is a third-party service that securely processes customer payments via credit cards, ACH, or other methods - such as Stripe Billing.
- **Pricing table** - A pricing table is a visual representation of your different subscription plans, showcasing features and prices to help users compare options. You can build one in Kinde and then show it to your customers in the auth flow or self-service portal.
- **Self-serve account portal** - Kinde provides a self-serve portal. This allows customers to manage their subscriptions—such as upgrading plans, updating payment information, or cancelling. This reduces support overhead and improves user autonomy. In Kinde, you can decide what your customers can self-manage or remove the functionality completely.

## Common billing terms

- **Agreement** - the term used in Stripe and the Kinde API to refer to a customer's subscription. Both terms are used throughout docs.
- **Base price** – The starting cost of a subscription plan, typically the cost for a core set of features or a minimum level of usage.
- **Chargeable / Non-chargeable feature** –
  - **Chargeable feature**: Incurs an additional fee when used. Might be metered or per unit price.
  - **Non-chargeable feature**: Add non-chargeable features for any included features which aren't independently chargeable, but need to be gated in your product.
- **Feature** – A specific function or capability of your SaaS product that you provision for app users. In context with a plan, these are chargeable or non-chargeable features that are provisioned to customers.
- **Fixed charge** – A recurring fee that does not change based on usage, often applied monthly or annually. Use this for a plan’s base price or other flat recurring fees.
- **Metered and unmetered feature** – Metered features are provisioned in units, often with pricing per unit, e.g. MAU. An unmetered feature is like a boolean, and is a basic feature with no pricing attached.
- **Multi-currency** – The ability to set plan prices in different currencies to support global customers. Kinde supports nearly all currencies, but you can currently only pick one as the default for all plans.
- **Plan** – A packaged offering of features, prices, and terms available for subscription, typically tiered across a group (e.g., Basic, Pro, Enterprise).
- **Plan group** – A collection of related plans, often grouped by customer type or usage level, allowing easier management and comparison.
- **Pricing model** – The structure used to determine how features in a plan are priced. E.g. fixed charge, tiered, per-user, or usage-based pricing. Kinde lets you use multiple pricing models within one plan.
- **Subscription** – The ongoing agreement where a customer pays for access to a SaaS product via a recurring plan. This is referred to as an agreement in Stripe.
- **Tiered pricing** – This refers to unit pricing that has different unit costs based on the volume of units purchased. E.g. $10 per unit for 1–10 units, $8 per unit for 11–50 units, $5 per unit over 51 units.
- **Unit price** – Where a price is set per unit of usage, e.g. a seat or license. Unit prices can also be applied to metered features, e.g. x per unit. Unit prices can also be tiered, e.g. x per unit up to 10 units, then y for 10+ units.
- **Usage-based** **price** – A billing method where charges vary based on the metered consumption of resources or services (e.g., API calls, storage).
