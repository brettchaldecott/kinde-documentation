
It’s common practice to override feature flag values for specific environments or for specific organizations or users. This might be done to enable feature testing, to analyse the effectiveness of a change, or to give early access to a preferred customer.

Feature flags values cascade from business to environment to organization to user. But overrides can be applied if they are available.

## Check if a flag override is available

1. Go to **Releases > Feature flags**.
2. Find the flag in the list.
3. Check the column indicating where overrides can be applied.

<img
  src="https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/a0c63a40-d3ae-4cde-31cd-4648569ff300/public"
  alt="The column showing where an override can be applied"
  width="672px"
  height="auto"
  fetchpriority="low"
  loading="lazy"
  decoding="async"
/>

    If overrides are allowed, follow one of the below procedures.

## Override feature flag value in an environment

When you apply an override in an environment, it will be applied to all users in the environment.

1. Go to **Settings > Environment > Feature flags**. All the feature flags are listed.
2. Find the flag you want to update and select **Edit value**. You can only do this if the flag value can be overridden for environments.
3. In the **Edit feature flag** window:
   1. For Boolean flag types, select the new value and then select **Save**.
   2. For string, integer, or JSON flag types, enter the new value and then select **Save**.
4. If you want to revert to the inherit default value, in the **Edit feature flag** window:
   1. For Boolean flag types, select the default value and then select **Save**.
   2. For string, integer, or JSON flag types, enter the new value and then select **Save**.

## Override feature flag value for an organization

When you apply a value override for an organization, it will be applied for all users in the organization.

1. Go to **Organizations** and select the organization. The **Organization details** window opens.
2. Select **Feature flags**. All the feature flags are listed.
3. Find the flag you want to update and select **Edit value**. You can only do this if the flag value can be overridden for organizations.
4. In the **Edit feature flag** window:
   1. For Boolean flag types, select the new value and then select **Save**.
   2. For string, integer, or JSON flag types, enter the new value and then select **Save**.
5. If you want to revert to the inherit default value, in the **Edit feature flag** window:
   1. For Boolean flag types, select the default value and then select **Save**.
   2. For string, integer, or JSON flag types, enter the new value and then select **Save**.

## Override feature flag value for a user

When you apply a value override for a user, it will be applied for that user across all organizations.

1. Go to **Users** and select the user. Their profile opens.
2. Select **Feature flags**. All the feature flags available to this user are listed.
3. Find the flag you want to update and select **Edit value**. You can only do this if the flag value can be overridden for users.
4. In the **Edit feature flag** window:
   1. For Boolean flag types, select the new value and then select **Save**.
   2. For string, integer, or JSON flag types, enter the new value and then select **Save**.
5. If you want to revert to the inherit default value, in the **Edit feature flag** window:
   1. For Boolean flag types, select the default value and then select **Save**.
   2. For string, integer, or JSON flag types, enter the new value and then select **Save**.
