
You can add and manage a user’s identity in Kinde. This includes their Kinde profile, contact, and sign in identity information. [Learn more about identities in Kinde](/authenticate/about-auth/identity-and-verification/).

This topic explains how to add profile identities manually in Kinde - you might do it this way as a one-off task.

You can also add identities via [the Kinde Management API](/kinde-apis/management/). You might do it this way for bulk processes.

<Aside>

Users with enterprise identities in Kinde can't also have other identity types in Kinde. E.g. a user can have an email identity and a social identity. But if a user has an enterprise identity, they cannot have other identities.

</Aside>

## Add an identity

1. In Kinde, go to **Users**.
2. Search or browse for the user you want to update.
3. In the **Profile** page for the user, in the **Identities** section, select **Add identity**.
4. In the dialog, choose the identity type and enter the details.
5. Select **Save**.

Create a username, email or phone identity via API: `POST /users/{user_id}/identities`

## Delete an identity

You can’t edit an identity record as it can break authentication for a user. Instead, add a new identity before you delete the old one.

1. In Kinde, go to **Users**.
2. Search or browse for the user you want to update.
3. In the **Profile** page for the user, in the **Identities** section, locate the identity you want to delete.
4. Select the three dots menu and select **Delete identity**.

Delete a username, email or phone identity via API: `DELETE /identities/{identity_id}`

## Make a contact identity primary

Primary contact information gets passed in the ID token for the user. You can change the primary identity.

1. In Kinde, go to **Users**.
2. Search or browse for the user you want to update.
3. In the **Profile** page for the user, in the **Identities** section, locate the identity you want to make primary.
4. Select the three dots menu and select **Make primary**.

Make an identity the primary one (`is_primary`) via API: `PATCH /identities/{identity_id}`

## Update a user identity

You can’t directly edit identities in Kinde. You need to add a new identity and delete the old one to update identity records.

## Cases in usernames

Kinde treats usernames as case-insensitive. In other words, we ignore case. We do this because it eliminates the possibility of auth issues and fraud when two usernames are identical in every aspect except the case of one of their letters. 

We are happy to support users choosing an aesthetically pleasing username combination, like `RosyRose` or `BuilderBob`. We just don't also support separate identities for `rosYrosE` and `BUilderbob`. Before adding users, we recommend checking that all usernames are unique in more than just case.
