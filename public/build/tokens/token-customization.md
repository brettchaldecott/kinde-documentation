
If you want to include additional information in tokens, you can customize access tokens, M2M tokens, and ID tokens using preset custom claims and [properties](/properties/about-properties/). If you need Kinde token formats to be third-party friendly, you can also enable mapping for those services, e.g. Hasura.

Token customization is a step toward enabling custom claims, but it is not the same thing.

## Add claims to a token

1. In Kinde, go to **Settings > Applications** and select **Details** on your application.
2. Select **Tokens**, then scroll to the **Token customization** section.
3. On the relevant token type card, select **Customize**. A window appears where you can select scopes and properties.

![Customize access token window with selection of scopes and properties](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/8b20408b-5494-4da9-c59c-01be32accf00/public)

4. Select the available **Additional claims** and **Properties** you want to include in the token.
5. Select **Save**.

## Additional claims for tokens
Apart from your own [custom properties](/properties/work-with-properties/properties-in-tokens/), you can add some out of the box additional claims to tokens.

### Add claims to access tokens

You can:
- add an organization name to an access token (`org_name`)
- add roles to an access token (`roles`)
- add an email to an access token (`email`)
- add an external organization ID to an access token (`external_organization_id`)

### Add claims to ID tokens

You can:
- Add a user's social identity to an ID token 
- Add a user's organizations to an ID token

### Add claims to M2M tokens

You can [add feature flags to an M2M token](/machine-to-machine-applications/m2m-application-setup/add-feature-flags-to-m2m-applications/).

## Add properties to tokens

To further customize tokens you can add information using [properties](/properties/about-properties/). 

Properties are custom fields and information that you can attach to your users, organizations, and applications in order to collect the data you want. For example, if you ask your customers to complete a form, you can map the answers to their user record. Another example could be you want to collect a delivery address for an organization, or include unique identifiers for M2M apps.

To make a property available in a token, you need to [make the property public](/properties/work-with-properties/manage-properties/), and then customize the token following the procedure above, to add a property. 

The value will appear in the token under a `application_properties` claim:

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

The `v` is a shortcode for the value of the property.

Only the properties you explicitly toggle on will be included.

## Token integration for third parties

Currently, we only support formatting for Hasura.

1. In Kinde, go to **Settings > Applications** and select **Details** on your application.
2. Select **Tokens.**
3. Scroll to the **Token integrations** section and switch the toggle on for the platform you use.
4. Select **Save**.
