
Trigger: `user:existing_password_provided`

This trigger fires after an existing password is entered in the sign-in flow.

## Security considerations

Security is at the heart of our technical decisions at Kinde, and keeping user passwords safe is a huge part of this. Therefore:

- Any attempt to log the password out to the console in this workflow will be redacted
- API calls should only be made from these workflows using the Kinde provided [secureFetch](/workflows/bindings/secure-fetch-binding/) binding which secures the payload with an encryption key

## Example use cases

### Drip feed migration

For gradual migrations to Kinde where you wish to check the password against an external database before creating the user in Kinde and migrating their password. [See example code](https://github.com/kinde-starter-kits/workflow-examples/blob/main/existingPassword/dripFeedMigrationWorkflow.ts)

## Workflow code

### Sample event object

The main argument provided to your code is the Kinde workflow `event` object which has two keys `request` and `context`. This gives you access to the reason the workflow was triggered. Here's an example:

```json
{
  "request": {
    "ip": "192.168.0.1",
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:135.0) Gecko/20100101 Firefox/135.0"
  },
  "context": {
    "domains": {
      "kindeDomain": "https://example.kinde.com" // Your Kinde domain
    },
    "auth": {
      "providedEmail": "hello@example.com", // the email provided by the user
      "password": "someSecurePassword", // the raw password
      "hashedPassword": "someHash", // the hashed password,
      "hasUserRecordInKinde": false // whether the user exists already in Kinde
    },
    "user": {
      "id": "kp_1234566" // only provided in password reset flows as otherwise new user
    },
    "workflow": {
      "trigger": "user:existing_password_provided"
    }
  }
}
```

### Secure fetch binding

We recommend you use the [secureFetch](/workflows/bindings/secure-fetch-binding/) binding to make API calls from your workflow if they include sensitive data like passwords.

### Widget binding

The `kinde.widget` binding gives you access to the Kinde widget, which is the central form on the page. In this case the form with the two password fields.

It exposes a method for invalidating a form field `invalidateFormField`

```js
kinde.widget.invalidateFormField(fieldName, message);
```

Example

```js
if (!isUserPasswordValid) {
  kinde.widget.invalidateFormField("p_password", "User or password not found");
}
```

The field names for the widget binding in this workflow are:

| Field name   | Description        |
| ------------ | ------------------ |
| `p_password` | The password field |

### Example workflows

See examples on GitHub:

[Drip feed migration](https://github.com/kinde-starter-kits/workflow-examples/blob/main/existingPassword/dripFeedMigrationWorkflow.ts) - Shows how to check a password against an external database before creating the user in Kinde.
