
Trigger: `user:plan_cancellation_request`

This trigger fires when a plan cancellation request is made. This can be initiated by the user through the Kinde self-serve portal or via the Kinde Management API.

This is not a deprovisioning workflow, this occurs before a cancellation request is processed.

This event is triggered when:

- the user asks to cancel their plan in the Kinde self-serve portal
- plan cancellation is initiated via the Kinde Management API

## Example use cases

### Check if customer is eligible to cancel

You may want to check if the customer is eligible to cancel their plan based on certain criteria, such as outstanding payments or minimum contract periods. If they are not eligible, you can prevent the cancellation and notify them.

## Workflow code

### Plan binding

The [kinde.plan](/workflows/bindings/plan-binding/) binding is used for plan related actions, such as denying a cancellation request.

### Sample event object

The main argument provided to your code is the Kinde workflow `event` object which has two keys `request` and `context`. This gives you access to the reason the workflow was triggered. Here's an example:

```js
{
  "request": {
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:135.0) Gecko/20100101 Firefox/135.0",
    "ip": "192.168.0.1"
  },
  "context": {
    "domains": {
      "kindeDomain": "https://example.kinde.com" // Your Kinde domain
    },
    "billing": {
      "currentPlanCode": "pro",
      "agreementId": "agreement_01971a7c5c7bf2d05888a8d6c77d08ce" // the subscription ID in Kinde
    },
    "workflow": {
      "trigger": "user:plan_cancellation_request"
    }
  }
}
```

### Example workflows

See examples on GitHub:

[Deny cancellation request](https://github.com/kinde-starter-kits/workflow-examples/blob/main/planCancellationRequest/denyPlanCancellation.ts) - Deny a plan cancellation request from a user.
