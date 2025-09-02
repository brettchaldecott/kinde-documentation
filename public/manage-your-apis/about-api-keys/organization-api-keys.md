
Organization-level API keys provide enhanced security by ensuring that all API calls are associated with a specific organization context. This is particularly useful for multi-tenant applications where you need to maintain clear boundaries between different organizations.

## What are organization-level API keys?

Organization-level API keys are API keys that are tied to a specific organization within your Kinde account. When these keys are used to authenticate, the resulting verification includes the organization context, ensuring that all API operations can be properly scoped to that organization.

## Benefits of organization-level API keys

### Security

- **Clear ownership**: All API calls are clearly associated with an organization
- **Reduced risk**: Prevents accidental cross-organization data access
- **Audit trail**: Clear tracking of which organization each API call affects

### Compliance

- **Data isolation**: Helps ensure compliance with data residency requirements
- **Access control**: Maintains clear boundaries for sensitive data
- **Regulatory compliance**: Helps meet industry-specific compliance requirements

### Multi-tenancy

- **Tenant isolation**: Perfect for SaaS applications serving multiple customers
- **Resource management**: Clear separation of resources between organizations
- **Billing isolation**: Separate tracking and billing per organization

### Let organizations self-serve

You can expose API key creation to organizations in the Kinde self-serve portal. See [Self-serve API keys](/manage-your-apis/add-manage-api-keys/self-serve-api-keys/) for more information.

## Organization context in verification

When you use an organization-level API key, the verification response automatically includes the `org_code` field:

```json
{
  "code": "API_KEY_VERIFIED",
  "message": "API key verified",
  "is_valid": true,
  "key_id": "api_key_123",
  "status": "active",
  "scopes": ["read:users", "write:users"],
  "org_code": "org_abc123",
  "user_id": null,
  "last_verified_on": "2024-11-18T13:32:03+11",
  "verification_count": 42
}
```

This `org_code` field is trusted and cannot be tampered with, ensuring that your application can rely on it for authorization decisions.

## Use cases for organization-level API keys

### SaaS applications

- **Customer integrations**: Each customer gets their own API key.
- **Data isolation**: Customer data is automatically separated.
- **Customization**: Customer-specific configurations and features.

### Internal tools

- **Department access**: Different departments get different API keys.
- **Project isolation**: Separate keys for different projects.
- **Resource management**: Control access to organization resources.

### Partner integrations

- **Third-party access**: Grant partners access to specific APIs.
- **Limited scope**: Control what partners can access.
- **Audit trails**: Track all partner API usage.

### AI and automation

- **AI assistants**: AI tools that help with organization-specific data.
- **Automated workflows**: Scripts that operate within organization boundaries.
- **Integration tools**: Tools that connect with organization systems.

## Best practices for organization-level API keys

### Key management

- **Naming conventions**: Use clear names that indicate the organization.
- **Scope limits**: Only grant the minimum required scopes.
- **Regular rotation**: Rotate keys periodically for security.
- **Monitoring**: Track usage patterns for each organization.

### Security considerations

- **Organization validation**: Always verify the `org_code` in verification responses.
- **Scope enforcement**: Enforce scope restrictions in your API.
- **Audit logging**: Log all API operations with organization context.
- **Access reviews**: Regularly review organization access patterns.

### Integration patterns

- **Multi-tenant APIs**: Design your APIs to handle organization context.
- **Data isolation**: Ensure data is properly isolated between organizations.
- **Error handling**: Provide clear error messages for organization-related issues.
- **Documentation**: Document organization-specific API behavior.
