
Kinde's billing webhooks are outbound calls Kinde makes to your specified endpoint when particular billing events occur. You can use them to keep your application synchronized with Kinde's billing events, trigger automated actions to provide a seamless experience for your customers, and reduce the load on your support team. 

## How Kinde webhooks work

Kinde webhooks use HTTPS REST to send information about events to a verified external URL that you provide. When an event is triggered, Kinde dispatches a JSON Web Token (JWT) containing relevant data to your registered endpoint. It's essential to verify the authenticity of this JWT to confirm the request originated from Kinde and then decode it to access the event data. If your endpoint doesn't return a 200 OK response, Kinde will retry the webhook call using a back-off policy. 

More about webhooks: Manage [Kinde webhooks](/integrate/webhooks/about-webhooks/) via the Kinde interface, or via the [Kinde Management API](https://docs.kinde.com/kinde-apis/management/#tag/webhooks).

## Billing webhook triggers and examples

### A customer cancels their subscription
* **Trigger**:`customer.agreement_cancelled`
* **Description**: Triggered when a customer subscription is cancelled, this might be by the customer or by an admin
* **Example**: Automatically deactivate premium features for a user in your application, initiate an offboarding sequence, or update their status in your CRM to "cancelled.

### A new agreement is created for a customer
- **Trigger:** `customer.agreement_created`
- **Description**: Triggered when a customer signs up to a plan or changes plans, and an agreement is created for the customer.
- **Example**: Provision new services for the customer, send a welcome email confirming their subscription, or grant access to exclusive content.

### A customer's invoice becomes overdue
* **Trigger:** `customer.invoice_overdue`
* **Description**: Triggered when a customer's invoice becomes overdue. Usually only triggered in cases where payment is not automated.
* **Example**: Send automated reminders to the customer about their outstanding payment, trigger a temporary suspension of services, or notify your accounts receivable team for follow-up.

### Metered usaage is recorded against a plan feature
* **Trigger:** `customer.meter_usage_updated`
* **Description**: Triggered when a customer's metered usage data is updated.
* **Example**: Ideal for displaying real-time usage statistics to the customer within your app, calculating potential overage charges, or sending notifications if their usage approaches a predefined limit.

### A payment from a customer fails
* **Trigger:** `customer.payment_failed`
* **Description**: Triggered when a customer's payment attempt fails, for example due to insufficient funds or incorrect credit card details.
* **Example**: Initiate a dunning process, prompt the customer to update their payment method, or temporarily restrict access to features until payment is resolved.

### A payment from a customer succeeds
* **Trigger:** `customer.payment_succeeded`
* **Description**: Triggered when a customer's payment is successfully processed.
* **Example**: Confirm a payment receipt to the customer, unlock paid features, or record the successful transaction in your accounting system.

### A plan is associated with a customer in Kinde
* **Trigger:** `customer.plan_assigned`
* **Description**: Triggered when a customer is associated with a plan in Kinde.
* **Example**: Activate the features associated with the newly assigned plan in your application, send a plan activation email, or update the customer's profile with their new plan details.

### A customer's plan changes (upgrade or downgrade)
* **Trigger:** `customer.plan_changed`
* **Description**: Triggered when a customer's billing plan is changed (e.g., upgraded or downgraded).
* **Example**: Adjust the customer's feature access based on the new plan, update their billing cycle information, or notify relevant internal teams about the plan changes.
