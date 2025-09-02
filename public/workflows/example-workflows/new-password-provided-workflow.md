
Trigger: `user:new_password_provided`

This trigger fires after a new password is entered in either the sign-up flow or the reset password flow.

## Security considerations

Security is at the heart of our technical decisions at Kinde, and keeping user passwords safe is a huge part of this. Therefore:

- Any attempt to log the password out to the console in this workflow will be redacted
- API calls should only be made from these workflows using the Kinde provided [secureFetch](/workflows/bindings/secure-fetch-binding/) binding which secures the payload with an encryption key

## Example use cases

### Custom password strength policy

As a baseline, Kinde runs the following password checks:

- A password is supplied
- The password is at least 8 characters long
- The password does not exist in the 1,000,000 most common passwords

With this workflow you can add your own custom code to run additional checks, for example if your business requires a specific mix of upper / lower case characters, or inclusion of special characters. [See example code](https://github.com/kinde-starter-kits/workflow-examples/blob/main/newPassword/customPasswordValidationWorkflow.ts)

### Sync password updates with an external system

For gradual migrations to Kinde where several apps are in play (e.g. a mobile application and a web application), you might want to migrate web users first and mobile app users later. If users have access to both applications, then password resets on the web application would not be persisted to the legacy mobile app password store.

With this workflow you can securely send the password to your mobile app system in order to keep them in sync. [See example code](https://github.com/kinde-starter-kits/workflow-examples/blob/main/newPassword/securelySyncPasswordWorkflow.ts)

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
			"firstPassword": "someSecurePassword", // the first password entered
			"secondPassword": "someSecurePassword", // password match field
			"newPasswordReason": "reset" | "initial" // whether it is registration or reset
		},
		"user": {
				"id": "kp_1234566", // only provided in password reset flows as otherwise new user
				"email": "hello@example.com" // the email provided
		},
		"workflow": {
			"trigger": "user:new_password_provided"
		}
	}
}
```

## Bindings

### Secure fetch binding

We recommend you use the [secureFetch](/workflows/bindings/secure-fetch-binding/) binding to make API calls from your workflow if they include sensitive data like passwords.

### Widget binding

The `kinde.widget` binding gives you access to the Kinde widget which is the central form on the page. In this case the form with the two password fields.

It exposes a method for invalidating a form field `invalidateFormField`.

```js
kinde.widget.invalidateFormField(fieldName, message);
```

Example

```js
const isMinCharacters = context.auth.firstPassword.length >= 50;

if (!isMinCharacters) {
  kinde.widget.invalidateFormField(
    "p_first_password",
    "Provide a password at least 50 characters long
  );
}
```

The field names for the widget binding in this workflow are:

| Field name          | Description                                                                              |
| ------------------- | ---------------------------------------------------------------------------------------- |
| `p_first_password`  | The first password field                                                                 |
| `p_second_password` | The second password field, typically to check it matches the first to help prevent typos |

### Example workflows

See examples on GitHub:

- [Sync passwords to another system](https://github.com/kinde-starter-kits/workflow-examples/blob/main/newPassword/securelySyncPasswordWorkflow.ts) - Use encryption keys to securely keep passwords in sync between systems.
- [Custom password validation](https://github.com/kinde-starter-kits/workflow-examples/blob/main/newPassword/customPasswordValidationWorkflow.ts) - Shows how to validate a password against your own rules.
