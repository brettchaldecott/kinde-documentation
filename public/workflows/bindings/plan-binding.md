
The `kinde.plan` binding provides methods to use for various plan related events such as when a customer requests to cancel or change their plan.

## Available in workflows

- [user:plan_cancellation_request](/workflows/example-workflows/plan-cancellation-request-workflow/)
- [user:plan_selection](/workflows/example-workflows/plan-selection-workflow/)

## Configuration

```js
export const workflowSettings = {
  // ...other settings
  bindings: {"kinde.plan": {}}
};
```

## Methods

### `denySelection(reason: string, suggestions: string[]): void`

Prevent the user from changing their plan.

#### Using the Kinde infrastructure library (recommended)

The [Kinde infrastructure library](https://github.com/kinde-oss/infrastructure) provides a type-safe helper:

```js
import { denyPlanSelection } from "@kinde/infrastructure";

export default async function (event: onPlanSelectionEvent) {
  if (!canChangePlan()) {
    denyPlanSelection("You are not allowed to change your plan as you have outstanding payments");
  }
}
```

#### Using the low-level API

If you're not using the infrastructure library, you can call the underlying API directly:

```js
kinde.plan.denySelection(
  "You are not allowed to change your plan as you have outstanding payments
);
```

### `denyCancellation(reason: string): void`

Prevent the user from cancelling their plan.

#### Using the Kinde infrastructure library (recommended)

The [Kinde infrastructure library](https://github.com/kinde-oss/infrastructure) provides a type-safe helper:

```js
import { denyPlanCancellation } from "@kinde/infrastructure";

export default async function (event: onPlanCancellationRequestEvent) {
  if (!canChangePlan()) {
    denyPlanCancellation("You are not allowed to change your plan as you have outstanding payments");
  }
}
```

#### Using the low-level API

If you're not using the infrastructure library, you can call the underlying API directly:

```js
kinde.plan.denyCancellation(
  "You are not allowed to cancel your plan as you have outstanding payments
);
```
