
The `url` binding allows you to access the native JavaScript `URLSearchParams` API. This is useful for parsing and manipulating URLs in your workflow and is often used in conjunction with the [kinde.fetch](/workflows/bindings/fetch-binding/) binding.

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
  bindings: {
    url: {}
  }
};
```

## Usage

### Using the low-level API

Once the binding is enabled you can use `new URLSearchParams` directly.

```js
export default async function (event) {
  const {data: token} = await fetch(`https://someapi.com/api`, {
    method: "POST",
    responseFormat: "json",
    headers: {"Content-type": "application/x-www-form-urlencoded"},
    body: new URLSearchParams({a: "b", b: "c"})
  });
}
```
