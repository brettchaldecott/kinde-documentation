
You can use **Properties** in Kinde to add structured metadata to a Machine-to-Machine (M2M) application. These properties can store configuration or contextual data, such as `region`, `tier`, or `model_version`.

While properties can be applied to users and orgs, only properties scoped to **Applications** are relevant for M2M apps. 

This feature is useful for:

- Customizing tokens
- Driving logic in downstream systems
- Managing AI agent behavior
- Setting up per-customer configuration without hardcoding or external storage

If you're looking to include properties inside tokens, see [Customize the claims of an M2M token with properties](/machine-to-machine-applications/m2m-application-setup/add-metadata-to-an-m2m-application-with-properties/).

## Step 1: Define the property for the M2M application

Before you can assign a property to an app, you need to define it.

1. In Kinde, go to **Settings > Data Management > Properties**.
2. Select **Add property**.
3. Give the property a name.
4. Add a description and key (e.g. `tier`, `region`, `model_version`).
5. If you want a property to be included in tokens, switch off the **Private** option.
6. Choose **Applications** for where the property is to be used.
7. Select **Save**.

The property is now available across all M2M applications.

## Step 2: Assign the property to an M2M application

1. Open the relevant M2M application in Kinde. 
2. Go to the **Properties** menu.
3. Add or edit values for the available properties.

For example:

| Property      | Value |
| ------------- | ----- |
| region        | eu    |
| model_version | v2    |
| tier          | pro   |

These values are stored with the app but are not included in tokens unless explicitly enabled.

## Step 3: Include a property in an M2M token

1. Open the relevant M2M application in Kinde. 
2. Select **Tokens** in the menu.
3. Scroll to the **Token customization** section and select **Customize** on the **M2M token** tile. 
4. Under **Properties**, toggle on the properties you want included in the token.
5. Select **Save**.

The selected properties are embedded in the token under the `application_properties` claim:

```json
{
  "application_properties": {
    "region": {
      "v": "eu"
    },
    "tier": {
      "v": "pro"
    }
  }
}
```
