
For properties to be available in an application, user or organization record, they first need to exist in your business. This topic describes how to create and edit properties.

Follow the procedure below to:

- add properties for users
- add properties for organizations
- add properties for applications 

## Add a property

You can also create and update properties through the [Kinde Management API](/kinde-apis/management#tag/properties).

1. In Kinde, go to **Settings** **> Data management > Properties**.
2. Select **Add Property**.

![Top half of Add property window](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/14161413-5a55-4455-bdb7-87d00ae6d900/public)

3. Enter a **Name** for the property.
4. (Optional) Add a short description. Note that this description will appear when someone goes to edit the value of a property in a User or Organization record.
5. Assign a **Key** to the property. This needs to be unique so that you can identify it in your code. The key cannot be changed later. Note that any Kinde included properties start with `kp-`.
6. Select if the property needs to remain **Private**. If you switch this off, the property can be selected to be included in tokens. See [Add and manage properties in tokens](/properties/work-with-properties/properties-in-tokens/).

![Screen shot of botton half of property window](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/38738ae6-1f04-4973-0532-4653656c0f00/public)

7. Select if the property is for **Users**, **Applications**, or **Organizations**.
8. Select a **Category**. Use an existing one or create your own, see [Create property categories](/properties/work-with-properties/property-groups/).
9. Select the **Property type**. You can choose from Boolean, single line text, or multi line text. For more information, see below.
10. Select **Save**.

## Choosing a property type

- Boolean: For when you want to indicate a true or false status, e.g. `paid_membership="true"`
- Single line text: For including a short piece of info in a property, e.g. `membership_type="bronze"`
- Multi line text: For including more substantial information, e.g. an address or business description.

## Include properties in tokens

One of the powers of properties is being able to use them to [include custom claims in tokens](/properties/work-with-properties/properties-in-tokens/). To ensure a property can be used in tokens, make sure you switch off the **Private** option at step 6, in the procedure above.

## Edit or delete a property

You can change the name and some property settings, but not all attributes can be edited. For example, you cannot change the property key once it has been created. You can also only delete the properties you created, not Kinde-created properties.

<Aside type="warning">

Take care changing the availability of a property to be included in tokens, such as switching a property from public to private. This could cause a breaking change if the property is already used in tokens. See [Add and manage properties in tokens](/properties/work-with-properties/properties-in-tokens/).

</Aside>

1. In Kinde, go to **Settings** **> Data management > Properties**.
2. Search or scroll for the property you want and select the three dots next to the property.
3. To edit, Choose **Edit property**, make the changes you want and select **Save**.
4. To delete, select **Delete property** then confirm that you want to delete. 
