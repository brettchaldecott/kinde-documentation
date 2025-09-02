
If you're building AI-powered tools or agents that act on behalf of your customers, machine-to-machine (M2M) applications in Kinde offer a simple, secure way to authenticate them.

M2M apps are ideal for:

- Long-running background jobs
- Headless tools or scheduled workers
- AI assistants, agents, or orchestration layers

By scoping M2M apps to specific organizations, you ensure each token can only access the data and functionality appropriate to that customer.

## Example: AI assistant per organization

You’ve built a support assistant that uses a customer’s internal knowledge base to generate responses. Each org gets their own agent instance.

With Kinde, you can:

- Create one M2M app per organization
- Assign custom metadata (e.g. model version, language preference)
- Include feature flags in the token (e.g. `enable_agent_v2`)
- Enforce `org_code` in your API to isolate access

Your backend APIs validate the incoming token and respond accordingly — no need for separate credential logic or inline configuration.

## Example: Shared agent with dynamic tokens

If you deploy a shared service that rotates between customer workspaces, you can:

- Store the appropriate client credentials per org
- Request tokens using the client credentials flow
- Receive a token scoped to a single org, with configuration claims

This keeps your infrastructure lightweight while maintaining strict org isolation.

## Best practices

- Use [Properties](/machine-to-machine-applications/m2m-application-setup/add-metadata-to-an-m2m-application-with-properties/) to define custom configuration like `model_version` or `max_tokens`
- Use [Token customization](/machine-to-machine-applications/m2m-token-customization/customize-m2m-tokens/) to inject only the claims your AI service needs
- Enforce `org_code` and `scopes` server-side to prevent token misuse
- Rotate credentials regularly and assign minimal scopes per app
- Include feature flags to roll out new AI capabilities safely

## Sample token contents

```json
{
  "org_code": "org_123456789",
  "application_properties": {
    "model_version": {
      "v": "gpt-4"
    },
    "region": {
      "v": "eu"
    }
  },
  "feature_flags": {
    "agent-v2": {
      "t": "b",
      "v": true
    },
    "beta-tools": {
      "t": "b",
      "v": false
    }
  }
}
```

The `t` and `v` are short codes for the type and value of the feature flag.

- `t` = `type` (boolean, string, number)
- `v` = `value` (true | false, "beta", 1, etc.)

Only the feature flags you explicitly toggle on will be included.
