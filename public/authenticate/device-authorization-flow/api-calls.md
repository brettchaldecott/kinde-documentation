
Once you've received an access token from the device authorization flow, you can use it to call your protected APIs. This guide shows you how to validate tokens, handle scopes, and make authenticated API requests.

## Use the access token from the device authorization flow

The access token you receive from the device authorization flow is a standard OAuth 2.0 Bearer token. Include it in the `Authorization` header of your API requests:

```bash
curl -X GET https://your-api.com/protected-resource \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## Token validation in the device authorization flow

Before processing API requests, validate the access token to ensure it's valid and hasn't expired:

### Validate with Kinde's userinfo endpoint

```bash
curl -X GET https://<your-subdomain>.kinde.com/oauth2/v2/user_profile \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

**Success response**:

```json
{
  "sub": "kp_c3143a4b50ad43c88e541d9077681782",
  "provided_id": "some_external_id",
  "name": "John Snow",
  "given_name": "John",
  "family_name": "Snow",
  "updated_at": 1612345678,
  "email": "john.snow@example.com",
  "email_verified": true,
  "picture": "https://example.com/john_snow.jpg",
  "preferred_username": "john_snow",
  "id": "kp_c3143a4b50ad43c88e541d9077681782"
}
```

**Error response** (invalid token):

```json
{
  "error": "invalid_token",
  "error_description": "The access token is invalid or expired"
}
```

### Validate with your own API

You can also validate tokens in your own API by verifying the JWT signature and claims:

```javascript
+// Node.js example using jsonwebtoken with JWKS
+const jwt = require("jsonwebtoken");
+const jwksClient = require("jwks-rsa");
+
+const client = jwksClient({
+  jwksUri: "https://<your-subdomain>.kinde.com/.well-known/jwks"
+});
+
+function getKey(header, callback) {
+  client.getSigningKey(header.kid, (err, key) => {
+    const signingKey = key.publicKey || key.rsaPublicKey;
+    callback(null, signingKey);
+  });
+}
+
+function validateToken(token) {
+  return new Promise((resolve, reject) => {
+    jwt.verify(token, getKey, { algorithms: ["RS256"] }, (err, decoded) => {
+      if (err) {
+        resolve({ valid: false, error: err.message });
+      } else {
+        resolve({ valid: true, user: decoded });
+      }
+    });
+  });
+}
```

## Scope enforcement for device authorization

Access tokens include scopes that determine what resources the user can access. Check the required scopes before processing requests:

```javascript
// Example: Check if user has required scope
function hasRequiredScope(token, requiredScope) {
  const decoded = jwt.decode(token);
  const tokenScopes = decoded.scope.split(" ");
  return tokenScopes.includes(requiredScope);
}

// Usage
if (!hasRequiredScope(accessToken, "read:users")) {
  return res.status(403).json({error: "Insufficient scope"});
}
```

## Common API patterns for device authorization

### Protected resource endpoint

```javascript
// Express.js example
app.get("/api/protected-resource", authenticateToken, (req, res) => {
  // req.user contains the decoded token payload
  res.json({
    message: "Access granted",
    user: req.user
  });
});

function authenticateToken(req, res, next) {
  const authHeader = req.headers["authorization"];
  const token = authHeader && authHeader.split(" ")[1];

  if (!token) {
    return res.status(401).json({error: "Access token required"});
  }

  // Validate token with Kinde
  fetch("https://<your-subdomain>.kinde.com/oauth2/v2/user_profile", {
    headers: {
      Authorization: `Bearer ${token}`
    }
  })
    .then((response) => {
      if (!response.ok) {
        throw new Error("Invalid token");
      }
      return response.json();
    })
    .then((user) => {
      req.user = user;
      next();
    })
    .catch((error) => {
      return res.status(401).json({error: "Invalid token"});
    });
}
```

### Error handling for device authorization

Handle common token-related errors:

```javascript
function handleTokenError(res, error) {
  switch (error.error) {
    case "invalid_token":
      // Token is invalid or expired
      return res.status(401).json({error: "Please re-authenticate"});

    case "insufficient_scope":
      // Token doesn't have required permissions
      return res.status(403).json({error: "Insufficient permissions"});

    default:
      return res.status(500).json({error: "Authentication error"});
  }
}
```

## Security best practices for device authorization

### Token storage

- **Never store tokens in localStorage**: Use secure HTTP-only cookies or memory storage
- **Validate tokens server-side**: Always validate tokens on your backend, not just the client

### Rate limiting

Implement rate limiting for token validation requests:

```javascript
const rateLimit = require("express-rate-limit");

const tokenValidationLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP to 100 requests per windowMs
  message: "Too many token validation requests"
});

app.use("/api/protected-resource", tokenValidationLimiter);
```

### Logging and monitoring

Log authentication events for security monitoring:

```javascript
function logAuthEvent(token, action, success) {
  console.log({
    timestamp: new Date().toISOString(),
    action: action,
    success: success,
    userId: token.user_id,
    scopes: token.scope
  });
}
```

## Testing your API

Test your protected endpoints with the access token:

```bash
# Test with curl
curl -X GET https://your-api.com/protected-resource \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"

# Test with JavaScript
fetch('https://your-api.com/protected-resource', {
  headers: {
    'Authorization': 'Bearer YOUR_ACCESS_TOKEN'
  }
})
.then(response => response.json())
.then(data => console.log(data));
```
