
The `kinde.env` binding lets you access environment variables in your workflows that you have previously set up in the [Kinde admin area](/build/env-variables/add-manage-env-variables/) or via the [management API](https://docs.kinde.com/kinde-apis/management/#tag/environment-variables/post/api/v1/environment_variables).

## Methods

### `getEnvironmentVariable(key: string): {value: string, isSecret: boolean}`

Get the value of an environment variable. The value is returned as an object with two properties: `value` (the value of the variable) and `isSecret` (a boolean indicating whether the variable is a secret).

Secret variables will be redacted in the logs.

## Available in workflows

- [user:pre_registration](/workflows/example-workflows/pre-user-registration-workflow/)
- [user:post_authentication](/workflows/example-workflows/workflow-user-post-auth/)
- [m2m:token_generation](/workflows/example-workflows/m2m-token-generation-workflow/)
- [user:new_password_provided](/workflows/example-workflows/new-password-provided-workflow/)
- [user:existing_password_provided](/workflows/example-workflows/existing-password-provided-workflow/)
- [user:tokens_generation](/workflows/example-workflows/user-token-generation/)
- [user:pre_mfa](/workflows/example-workflows/pre-mfa-workflow/)
- [user:plan_selection](/workflows/example-workflows/plan-selection-workflow/)
- [user:plan_cancellation_request](/workflows/example-workflows/plan-cancellation-request-workflow/)

## Configuration

```js
export const workflowSettings = {
  // ...other settings
  bindings: {"kinde.env": {}}
};
```

## Usage

### Using the Kinde infrastructure library (recommended)

The [Kinde infrastructure library](https://github.com/kinde-oss/infrastructure) provides type-safe environment variable access:

```js
import {getEnvironmentVariable} from "@kinde/infrastructure";

const myVar = getEnvironmentVariable("MY_VAR")?.value;
```

### Using the low-level API

If you're not using the infrastructure library, you can call the underlying API directly:

```js
kinde.env.getEnvironmentVariable("MY_VAR")?.value;
```
