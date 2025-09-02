
Trigger: `user:tokens_generation`

This trigger fires when ID and Access tokens are generated for a user.

- After authentication - a sign in or sign up event.
- After a pre-authentication event - i.e the user is redirected to Kinde and returned invisibly to your app because they were already authenticated.
- When a refresh token is generated.

## Security considerations

To ensure the integrity of Kinde-issued tokens there are some claims which cannot be modified. See [Prohibited claims](https://github.com/kinde-oss/infrastructure/blob/main/lib/prohibitedClaims.ts) in the workflows infrastructure resource.

## Example use cases

Here's a few ways that this trigger might be used for a workflow.

### Custom claims using Kinde data

You may want to add additional custom claims to the access or ID token before it is delivered to your product.

### Custom claims using data from an external service

You have entitlements or other data in a CRM that you wish to make an API call out to, and then append that data into the access token. [See example code](https://github.com/kinde-starter-kits/workflow-examples/blob/main/userTokens/customClaimsAccessTokenWorkflow.ts)

### Reduce token size

Kinde automatically populates the access token with data such as `permissions` and `feature_flags`. If your product use these features heavily, the access token can bloat and you may prefer to use the Kinde Management API to get the data, and strip them from the token.

### Modify Kinde claim format

Some external systems rely on claims to be in a certain format. For example, Kinde supplies roles as an array of objects, but some systems require a space separated string. This workflow allows you to restructure the format of these Kinde claims.

## Workflow code

### Sample event object

The main argument provided to your code is the Kinde workflow `event` object which has two keys `request` and `context`. This gives you access to the reason the workflow was triggered. Here's an example:

```json
{
	"request": {
		"auth": {
			"audience": ["<EXAMPLE_API>"]
		},
		"ip": "192.168.0.1"
	},
	"context": {
		"domains": {
			"kindeDomain": "<https://example.kinde.com>" // Your Kinde domain
		},
		"auth": {
			"origin": "authorization_request" | "refresh_token_request",
			"isExistingSession": false, // if user was preauthenticated,
			"connectionId": "conn_12345" // the ID of the auth connection method used
		},
		"application": {
			"clientId": "299627bd8bfa493f8b17e6aec8ebfb86" // the M2M application ID
		},
		"user": {
			"id": "kp_1234567890", // the ID of the user
			"identityId": "identity_123456789" // the ID of the identity the user authenticated with
		},
		"organization": {
			"code": "org_123456789" // the org code the user authorized against
		},
		"workflow": {
			"trigger": "user:tokens_generation"
		}
	}
}
```

### Access and ID token bindings

To modify claims in the generated user tokens you will need to make use of the following bindings

- [kinde.accessToken](/workflows/bindings/access-token-binding/)
- [kinde.idToken](/workflows/bindings/id-token-binding/)

### Example workflows

See examples on GitHub:

[Add custom claims to access token](https://github.com/kinde-starter-kits/workflow-examples/blob/main/userTokens/customClaimsAccessTokenWorkflow.ts) - Call an external API to get data to add as custom claims to the user access token.
