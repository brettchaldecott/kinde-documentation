
You can build pricing tables to enable your customers to select plans and go through a payment flow as part of signing up to your app or site.

Kinde's pricing table builder can generate a pricing table from published plans, or you can start with a blank one. 

You can also add and edit information and content in your preferred languages. You can create as many pricing tables as you want.

![parts of a pricing table](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/c911b0af-5c1d-44f8-970b-47c0084ad500/public)

## Create a pricing table

A pricing table can only have 4 plans.

1. Go to **Billing > Pricing tables**.
2. Select **Add pricing table**. 
3. Choose an option:
    1. **Generate** a pricing table from a [plan group](/billing/manage-plans/add-manage-plan-groups/) - this pre-populates the pricing table with basic details and the plans based on the plan group you select. Up to 4 plans can be included on a pricing table. If your plan group contains more, you may want to create a new pricing table.
    2. **Create new** pricing table. Manually build the pricing table and add plans from a plan group. This gives you a blank slate to start in.
4. Select **Next**.
    1. **Generated** - select the group and then **Save**.
    2. **New** - complete the details in the window and **Save**.

## Edit the pricing table

You can edit a pricing table, but they are not versioned. Whatever content you override, it cannot be reverted or recovered.

You cannot edit a plan price. This is inherited from the plan itself.

## Change what plans show on a pricing table

You can add and remove plans from the pricing table when it is being worked on. 

1. Open the pricing table.
2. Scroll to the **Plans** section. 
3. To remove a plan:
    1. Select the three dots on the plan card and select **Remove from pricing table**.
    2. Confirm you want to remove the plan. This removes all custom content you have added, in all languages you have added content in, on the pricing table. The plan is removed. 
    3. Select **Save**. 
4. To add a plan, select **Add plan**. 
    1. In the window that opens, select an available plan. Only plans from the same plan group are shown.
    2. Complete the details in the window for this plan. At minimum, you must enter a **Display name** and the **CTA button** content. This will be displayed in the pricing table.
    3. Select **Save**. The plan is added to the pricing table.

## Change the order of plans on a pricing table

1. Open the pricing table.
2. Scroll to the **Plans** section. 
3. Select the three dots menu on the plan you want to move. 
4. Select **Move up** or **Move down**, depending on the plan position.
5. Select **Save**.

This is the order plans will be displayed left to right on the pricing table.

## Add a features list to a plan on the pricing table

To promote the top features in a plan, add them manually to the pricing table. These features don't need to correspond exactly to your plan configuration, this is an opportunity to sell the most appealing product features and make plan choices easy for customers.

1. Open the pricing table.
2. Scroll to the **Plans** section. 
3. Select the three dots menu on the plan and select **Edit content**. 
4. In the top of the window, select the language you want to add features in.
5. Add a **Features list heading**. This sits directly above the list.
6. Add a list of features in the large text **Features list** field. Features will appear in the order they are listed.
7. Select **Save**.

## Highlight a plan on a pricing table

It’s common to want to call out or highlight something about a plan on the pricing table, for example, to highlight which plan is most popular.

1. Open the pricing table.
2. Scroll to the **Plans** section. 
3. Select the three dots menu on the plan and select **Edit content.**
4. In the top of the window, select the language you want to add highlight content.
5. Add a **Highlight label** and select **Save**. 

## Edit and translate pricing table content

If you want to display pricing tables in multiple languages, you can change add content translations to the plans on the pricing table.

1. Open the pricing table.
2. Scroll to the **Plans** section. 
3. Select the three dots menu on the plan and select **Edit content.**
4. In the top of the window, select the language you want to add or edit content for.
    
    ![image.png](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/58587ad1-0077-47d7-faca-ea3dd6345d00/public)
    
5. Enter or edit all the content you want in the chosen language.
6. Select **Save**.

## Make the pricing table live to customers

We recommend only doing this after your plans are finalized and published.

1. Open the pricing table.
2. Select **Make live**. 
3. Select **Save**. If a user passes the pricing table code in a URL, they will be able to sign up using it.

Here's how to [add the pricing table params to URLs](/billing/billing-user-experience/add-billing-to-url-sdk/).

## Set pricing table to show as default in register flow

You can set a pricing table to show by default, even if the code key is not passed in the URL during the normal sign up flow.

1. Open the pricing table.
2. Select **Show by default**. 
3. Select **Save**. If a user goes through the registration flow without a pricing table code key in the URL, this is the pricing table they will see.

## Switch the pricing table off for the register flow

You can configure Kinde to hide pricing tables in the sign up flow, but still show them in the self-serve portal. You'd do this if you have designed your own pricing pages and you only want to show Kinde's pricing table for the self-serve upgrade or downgrade experience. 

1. Go to **Settings > Environment > Billing**.
2. Switch off the **Show the pricing table when customers sign up** option. 
3. Select **Save**. 

## Add plan selector to registration URL

Use your SDK or manually change URL params to incorporate billing. See [Add billing to URl or SDK](/billing/billing-user-experience/add-billing-to-url-sdk/)
