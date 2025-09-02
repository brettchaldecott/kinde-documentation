
Trigger: `m2m:token_generation`

This trigger fires when an M2M token is generated.

<Aside>

You cannot modify tokens when the Kinde management API has been requested as an audience.

</Aside>

## Example use cases

### Custom claims

You may want to add additional custom claims to the M2M token before it is delivered to your product.

### Correlate an M2M application with an organization or user

If you want, you can use M2M applications similar to API keys to enable access to various endpoints and tie them to an organization or user. For example, you add the organization code as a [custom property](/properties/work-with-properties/manage-properties/) on the M2M application, then fetch any data you’d like to include in the token. [See example code](https://github.com/kinde-starter-kits/workflow-examples/blob/main/m2mToken/mapOrgToM2MApplicationWorkflow.ts)

## Workflow code

### M2M token binding

The [kinde.m2mToken](/workflows/bindings/m2m-token-binding/) binding is used to modify claims in the generated access token.

### Sample event object

The main argument provided to your code is the Kinde workflow `event` object which has two keys `request` and `context`. This gives you access to the reason the workflow was triggered. Here's an example:

```json
{
  "request": {
    "auth": {
      "audience": ["<EXAMPLE_API>"],
      "scope": ["read:users"]
    },
    "ip": "192.168.0.1"
  },
  "context": {
    "domains": {
      "kindeDomain": "https://example.kinde.com" // Your Kinde domain
    },
    "application": {
      "clientId": "299627bd8bfa493f8b17e6aec8ebfb86" // the M2M application ID
    },
    "workflow": {
      "trigger": "m2m:token_generation"
    }
  }
}
```

### Example workflows

See examples on GitHub:

[Map M2M applications to organizations](https://github.com/kinde-starter-kits/workflow-examples/blob/main/m2mToken/mapOrgToM2MApplicationWorkflow.ts) - Shows how to map M2M applications to organizations. Useful if using Kinde for B2B API key management
