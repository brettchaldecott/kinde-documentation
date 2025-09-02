
You can include feature flags in the tokens issued to machine-to-machine (M2M) applications in Kinde. This is helpful for enabling or disabling functionality in downstream systems based on feature access.

<Aside>

At this time, only **environment-level feature flags** can be included in M2M tokens. Support for organization-assigned flag values may be added in future releases.

</Aside>

## Define environment-level feature flags

Before including a flag in a token, you need to define it in your environment. This is the quick procedure (below), here's the [detailed procedure for adding feature flags](/releases/feature-flags/add-feature-flag/). 

1. In Kinde, go to **Releases**.
2. Select **Add feature flag**.
3. Give the flag a key and (optionally) a description.
4. Choose the flag type (boolean, string, integer, JSON).
5. (Optional) Add a default value.
6. (Optional) Decide if the feature flag value can be overridden.
7. Select **Save**.

Once defined, this flag will be available for inclusion in any M2M token issued in the same environment.

## Include a flag in an M2M token

1. Go to **Settings > Applications > Your M2M app**.
2. Select **Tokens** in the menu.
3. Under **Feature flags**, toggle on the flags you want included in the token.

These flags will be embedded in the token under the `feature_flags` claim:

```json
{
  "feature_flags": {
      "new-ai-agent": {
          "t": "b",
          "v": true
       },
  	   "access-level": {
          "t": "s",
          "v": "beta"
       }
    }
}
```
The `t` and `v` are short codes for the type and value of the feature flag.

- `t` = `type` (boolean, string, number)
- `v` = `value` (true | false, "beta", 1, etc.)

Only the feature flags you explicitly toggle on will be included.

## Common use cases

- Enable or disable AI models or endpoints
- Drive conditional logic in APIs or job runners
- Gate functionality in distributed workers

## Notes

- Flags are set at the **environment** level - they are global, not org-specific
- Token customization is configured per-app in the **Tokens** tab
- Tokens remain small: only enabled flags are included
- Flag values are read-only for the recipient - you must update them via the dashboard or API
