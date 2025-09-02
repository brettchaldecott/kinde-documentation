
Workflows rely on your code being available to execute when the trigger event happens. When you update your code, you need to update the related workflow in Kinde.

## Sync code from your repo

Get the latest code from your repo so that it can be checked for deployment-readiness. The result of a sync will tell you if the code is ready.

1. Go to **Workflows**.
2. Select **Sync code**. This will update all workflows with any new code. The latest deployment shows on the **Summary** page for each workflow.
3. If the code is valid, Kinde will create a new deployment for each workflow.

## Make a deployment live

By default, Kinde will create a new deployment and set it live immediately when you sync your code.

If you have disabled this settings, you may wish to manually make the latest code deployment live, so it is used in the workflow. You can make any deployment live which can be useful for rolling back to working versions of your code if there are issues with your currently live version.

1. Go to **Workflows**.
2. Refresh code from your repo by selecting **Sync code**.
3. Select the workflow you want to update.
4. On the **Workflow** page, select **Make live** from the deployment context menu. The workflow will update to use the latest code.

## View all deployments

1. Go to **Workflows**.
2. Select the workflow to view the details.
3. Select **Deployments** in the menu. A list of all your deployments shows. See below to interpret deployment statuses.

## Deployment status

| Status      | Meaning                                                                                                                   |
| ----------- | ------------------------------------------------------------------------------------------------------------------------- |
| Live        | The code version currently live and ready to use in a workflow                                                            |
| Deployed    | Code that you have committed in your repo and has been checked for validity in Kinde and is ready to run in the workflow. |
| Failed      | Code that you have committed in your repo and has been found not be valid to run in Kinde.                                |
| Deactivated | Code that was live and has now been deactivated. It is possible for it to be made live again.                             |
| Processing  | You may occasionally see one of these. They indicate transitional states before a definitive status is achieved.          |
| Draft       | New workflow without a live deployment.                                                                                   |
