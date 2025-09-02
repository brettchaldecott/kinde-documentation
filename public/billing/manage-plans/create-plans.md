
Once you’ve got a plan strategy along with a plan feature list, prices, and details, you’re ready to create plans them in Kinde.

All plans must belong to a plan group. When you create your first plan, it will be automatically added to the default group. A plan group can only contain either user plans or organization plans. When you create your first plan, we automatically create a group based on the plan type you select. 

<Aside>

Remember you can create one charge or feature that can be used and redefined across all plans. E.g. You can have one ‘Base price’ charge that is defined as 0.00 in a free plan, 10.00 in a Plus plan, and 25.00 in a Pro plan.

</Aside>

## Add a plan

1. Go to **Billing > Plans**. 
2. If this is the first plan, select **Add plan** on the empty page. 
3. If you already have plans, select **Add plan** in the top right. The **Add plan** window opens.
    
    ![Add plans window](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/a8a237d5-c242-45dc-453e-a7ad66e33a00/public)
    
4. Choose whether the plan is for **Organizations** or **Users**, or select the **Group** the plan belongs to. Only one of these options will appear. You cannot change these selections later.
5. Give the plan a **Name** (e.g. `Free`), a **Description**. This is the name that will also appear in the pricing table (if you use one).
6. Give your plan a **Key** for referencing in your code. This cannot be changed after a plan is published.
7. Select a **Default currency**. This field only appears if one has not been set. You can change this later in **Settings > Billing**, but only before any of your plans are published.
8. Select **Save**.

## Add a subscription fee or fixed charge to a plan

A fixed charge is a recurring monthly charge such as base price or subscription fee.

<Aside type="warning">

You need to create a $0.00 charge for Free plans. This ensures the plan is added to Stripe, and is included on the pricing table.

</Aside>

1. Select **Add charge**.
    1. If the charge exists, select **Use existing feature** then select it from the list. 
        
        <Aside>
        
        You only need to create a new charge if the item is unique. For re-usable items such as ‘Base price’, select the existing charge but change the line item description and price for each plan.
        
        </Aside>
        
    2. If this is a new charge, give it a name (e.g., `Base subscription fee`). If you’ll reuse this charge across plans, choose a generic name. This cannot be changed later.
2. Add a **Line item description**. This appears on the customer invoice, so might be more specific than the name, e.g. `Base subscription - Pro plan`.

3. Set the price. (You may be asked to set a default currency if you haven’t already).
4. Select **Save**.
5. Select **Save draft**.
6. Repeat from step 1 for each fixed charge you want to add, or start adding features.

<Aside title="Feature price sync">

If you are successfully connected to Stripe and the charge is part of a published plan, it will show as `price synced`. Otherwise the charge will show as `price not synced`.

</Aside>

## Add features (entitlements) to a plan

Features describe the individual functions or entitlements that users get with their plan. These can be of the type metered (chargeable) or unmetered (included).

<Aside type="warning">

You cannot change the Name, the Key or the Type of feature once you have created it. If you make an error, delete the feature and add a new one. 

</Aside>

1. Select **Add feature**.
    
    ![Add feature window](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/ceb95ba9-1c03-4ae7-89f9-275ab3185100/public)
    
2. To add a new feature, select **New unmetered** or **New metered**, then select **Next**. The **Add feature** window opens.
3. Enter a **Feature name**. Be sure to use something generic if you plan to use this feature again, e.g. `Included seats`.
4. Enter a **Description**.
5. Enter a **Key** for referencing the feature. 
6. If you selected a unmetered feature, select **Save**. You’re done. Repeat from step 1 to add more features. 
7. If you selected a metered feature, complete the rest of the details:
    
    ![Add feature window, select units](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/de016f2c-2477-4abd-9a2d-00e7c3e99300/public)
    
8. Enter the **Maximum units** allowed on the plan. Leave blank if there is no limit. 
9. Enter the **Unit measurement** name, e.g. `license`, `MAU`
10. If the item is separately chargeable, select a **Pricing model**. 
    
    ![Add feature window, tiered prices](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/80ea9a38-e6ac-479f-9697-f04e237d4d00/public)
    
11. Add a **Line item description**. This appears on the customer invoice, so might be more specific than the name, e.g. `Additional licenses for Pro (5)`.
12. For a flat unit price, enter the price and select **Save**. You’re finished.
13. For tiered pricing, select **Tiered - Graduated** and define each tier with unit numbers and add the price per unit. 
14. Select **Add tier** to add a different price for higher unit tiers. For example, a unit might be $10 each if you use 1-10, but is $9 per unit if you use 11 or more.
    
    <Aside type="warning">
    
    Ensure the tiered units amounts don’t overlap. E.g. make sure they follow a pattern like 1-10, 11-20, 21-30, etc. If you want to make it so an upper limit is limitless, E.g. 1-10, 11-(blank/limitless); use 1-10, 11-1,000,000 (or a very high number). Graduated unit fields can’t be left blank.
    
    </Aside>
    
15. Select **Save.**
16. In the **Plan** window, select **Save** to commit your changes.
17. Repeat for each feature you want to add to the plan. Then repeat this procedure for each plan. 

<Aside title="Feature price sync">

If you are successfully connected to Stripe and the feature is part of a published plan, it will show as `price synced`. Otherwise the feature will show as `price not synced`.

</Aside>

**Next:** [Step 5 Publish plans](/billing/get-started/publish-plans/)
