
<Aside type="upgrade">

This feature is available on the [Kinde Plus and Kinde Scale plans](https://kinde.com/pricing/)

</Aside>

You can create machine-to-machine (M2M) applications in Kinde that are scoped to a specific organization. These applications are used to issue access tokens that are automatically restricted to that organization’s context, ensuring strong tenant isolation when calling Kinde’s APIs or your own.

Org-scoped apps use the same client credentials flow as other M2M apps, but the tokens they generate include the `org_code` claim.

## When to use an org-scoped M2M application

Use an org-scoped app when:

- You're building automation or AI tools that act on behalf of a specific customer or tenant
- You want to issue per-organization API access keys with enforced access boundaries
- You need to simplify your API logic by trusting `org_code` directly from the token
- You want tighter auditability and control across multiple customer environments

## Token structure

Tokens issued to an org-scoped app include trusted claims such as:

- `org_code`: the organization the token is scoped to
- `scope`: the permissions granted to the token

```json
{
  "org_code": "org_123",
  "scope": "read:users write:flags"
}
```

These claims are enforced at token issuance time and cannot be modified by the caller.

## Create an org-scoped M2M app

1. In Kinde, go to **Organizations**, then view an organization.
2. Select **M2M apps**.
3. Enter a name for the application.
4. Select **M2M application** as the type.
5. Select **Save**.

Kinde generates a `client_id` and `client_secret` tied to the selected organization.
Use the credentials in a standard client credentials flow to request a token.

Use the credentials to authenticate using the [client credentials flow](/machine-to-machine-applications/about-m2m/authenticate-with-m2m/). The resulting token will include the organization context automatically.

These claims can be used by your backend services to authorize access to specific APIs or resources.

## Org-scoped vs global M2M applications

| Feature               | Global M2M app                     | Org-scoped M2M app             |
| --------------------- | ---------------------------------- | ------------------------------ |
| Org context in token  | No                                 | Yes                            |
| Tenant data isolation | Manual                             | Enforced                       |
| Use case              | Admin scripts, internal automation | Per-tenant agents, scoped APIs |
| Token restrictions    | None                               | Scoped to one org              |
| Token claims          | Basic                              | Includes `org_code`            |

## Best practices for org-scoped M2M apps

- Use separate M2M apps for different scopes or services.
- Limit the [scopes](https://docs.kinde.com/developer-tools/your-apis/custom-api-scopes/) assigned to each M2M app to the minimum required for its function.
- [Rotate client secrets](/build/applications/rotate-client-secret/) periodically using the UI.
- Audit token usage by tracking `client_id` and `org_code` in logs.
- Avoid including any personally identifiable information (PII) in token claims.
