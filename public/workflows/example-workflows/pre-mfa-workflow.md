
<Aside type="upgrade">

This workflow trigger is only available on the Kinde Scale plan

</Aside>

Trigger: `user:pre_mfa`

This trigger is fired after the user has complete single factor authentication (e.g email + password or Google) and determined which organization (if any) they are trying to access.

## Example use cases

### Determining if MFA is required based on data in an external service

You may be using a service like Zanzibar or OpenFGA for fine-grained access control and wish to call out to it to determine if a user needs to complete MFA.

### Making MFA required if a user has a certain permission

You may wish to enforce MFA for users who have some sensitive permissions such as `delete:project` .

### Skipping MFA for certain organizations

You want to enforce MFA at an environment level for all organizations, but there are a few who do not want to adopt your policy.

### MFA prompt grace period

When you only want to prompt MFA when a user has not been asked for MFA for a certain time period. [See example code](https://github.com/kinde-starter-kits/workflow-examples/blob/main/preMFA/gracePeriodWorkflow.ts)

## Workflow code

### Sample event object

The main argument provided to your code is the Kinde workflow `event` object which has two keys `request` and `context`. This gives you access to the reason the workflow was triggered and additional relevant datapoints. Here’s an example:

```json
{
  "request": {
    "ip": "192.168.0.1",
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:135.0) Gecko/20100101 Firefox/135.0"
  },
  "context": {
    "auth": {
      "connectionId": "conn_01945d3ccf4926118bfdeb6e1158edb5" // connection ID the user auth'd with
    },
    "domains": {
      "kindeDomain": "https://example.kinde.com"
    },
    "mfa": {
      "policy": "required", // required | optional | off
      "context": "environment", // environment or organization
      "enabledFactors": ["mfa:sms", "mfa:email", "mfa:authenticator_app"], // factors you have enabled for this context
      "isUserRoleExempt": false, // advanced orgs can make specific roles exempt from MFA
      "isUserConnectionExempt": false // advanced orgs can make enterprise connections exempt from MFA
    },
    "user": {
      "id": "kp_77d28fc7b16240dd9ec12b08071fe46e" // the users ID
    },
    "workflow": {
      "trigger": "user:pre_mfa"
    },
    "application": {
      "clientId": "cee9743fc7ee4d2e84061fe1a662051d"
    },
    "organization": {
      "code": "org_75ad9f26d2c"
    }
  }
}
```

## Bindings

### MFA binding

The [kinde.mfa](/workflows/bindings/mfa-binding/) binding is used to modify the MFA policy for the current auth flow.

### Example workflows

See examples on GitHub:

[Set a grace period for MFA](https://github.com/kinde-starter-kits/workflow-examples/blob/main/preMFA/gracePeriodWorkflow.ts) - Don't ask for MFA for a set period of time after a user has logged in.
