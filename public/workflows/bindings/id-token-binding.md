
The `kinde.idToken` binding lets you modify ID tokens for the current user. When you add custom claims, these are included in the ID token returned to the client.

## Methods

### `setCustomClaim(name: string, value: any): void`

Adds a custom claim to the ID token. The claim will be available when the token is returned to the client.

## Available in workflows

- [user:tokens_generation](/workflows/example-workflows/user-token-generation/)

## Configuration

```js
export const workflowSettings = {
  // ...other settings
  bindings: {"kinde.idToken": {}}
};
```

## Usage

### Using the Kinde infrastructure library (recommended)

The [Kinde infrastructure library](https://github.com/kinde-oss/infrastructure) provides type-safe ID token modification:

```js
import { idTokenCustomClaims } from "@kinde/infrastructure";

export default async function (event: onUserTokenGeneratedEvent) {
  const idToken = idTokenCustomClaims<{
    userRole: string;
    lastLogin: string;
  }>();

  idToken.userRole = "admin";
  idToken.lastLogin = new Date().toISOString();
};
```

### Using the low-level API

If you're not using the infrastructure library, you can modify the ID token directly:

```js
kinde.idToken.setCustomClaim("userRole", "admin");
kinde.idToken.setCustomClaim("lastLogin", new Date().toISOString());
```
