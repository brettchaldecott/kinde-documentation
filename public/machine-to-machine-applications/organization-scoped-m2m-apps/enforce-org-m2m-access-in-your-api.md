
If you're using [org-scoped machine-to-machine (M2M) apps](/machine-to-machine-applications/organization-scoped-m2m-apps/m2m-applications-for-organizations/), Kinde will automatically include a trusted `org_code` claim in the token. You can then enforce access control in your own APIs using this claim.

## When to enforce org context

You should validate `org_code` in any API route or resource that is **tenant-specific**, such as:

- `/orgs/:org_code/users`
- `/reports/:org_code/usage`
- Backend functions or queues scoped to a customer’s data

If the token’s org doesn’t match the route or request context, you should reject the request.

## What to check

When you receive a token, decode it and check the following.

1. The token is valid and not expired.
2. The token contains `org_code`.
3. The `org_code` in the token matches the organization being accessed.

## Example in Node.js (Express)

```js
const jwt = require("jsonwebtoken");
function verifyOrgAccess(req, res, next) {
  const authHeader = req.headers.authorization;
  const token = authHeader?.split(" ")[1];
  if (!token) return res.status(401).send("Missing token");
  // If Kinde is **not** validating the token for you,
  // verify the signature instead of a raw decode.
  // const PUBLIC_KEY = ... // fetch from /.well-known/openid-configuration
  const decoded = jwt.verify(token, PUBLIC_KEY, {algorithms: ["RS256"]});
  // Check for required claims
  if (!decoded?.org_code) return res.status(403).send("Token not scoped to an organization");
  const orgFromRoute = req.params.org_code;
  if (decoded.org_code !== orgFromRoute) {
    return res.status(403).send("Token does not match organization");
  }
  next();
}
```

Then apply the middleware:

```js
app.get("/orgs/:org_code/users", verifyOrgAccess, (req, res) => {
  // safe to fetch users for this org
});
```

## Tips for working with org scoped M2M apps accessing your API

- You don’t need to verify the token signature if Kinde is validating the token on the backend (e.g. if you’re using our hosted APIs).
- If you’re validating the token yourself, use the public key from your environment’s OpenID configuration:
  `https://<your-subdomain>.kinde.com/.well-known/openid-configuration`
- Always treat `org_code` as a **binding contract** — never override it with user input

## How to treat global tokens

If the token has no `org_code`, treat it as **not authorized** to access org-specific resources unless explicitly allowed.

You may choose to:

- Reject the request
- Allow access only to system-level endpoints
- Prompt developers to use org-scoped apps instead
