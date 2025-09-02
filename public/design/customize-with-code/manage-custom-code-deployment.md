
Each time your code is synced to Kinde, a new deployment is created. A deployment is a unique point-in-time process that checks the code, which can then be made live.

## Enable code preview

Kinde Plus and Scale plan customers are able to preview the design before setting the code live in the production environment.

In **Settings > Git repo**, switch on the **Enable preview mode** option.

If this setting is not available to you as a Free or Pro plan user, we suggest you use non-production environments to check your designs before deploying to production.

## Sync code

Every time you change your code in GitHub, you must sync those changes back to Kinde so you can make them live.

1. Go to **Design > Custom code**.
2. Select **Sync code**. A new deployment is generated on click. If the deployment is successful, you will be able to make it live.

## Make a deployment live

1. Go to **Design > Custom code**. The **Summary** page shows which deployment is live and which is ready to go.
2. If you want, select **Sync code** to get the latest deployment.
3. Select **Deployments** in the menu. A list of current and previous deployments is shown.
4. Find the deployment you want to make live and select **Make live** in the three dots menu.

## Roll back a deployment

To roll back a deployment, you just need to make a previous deployment live.

1. Go to **Design > Custom code**. The **Summary** page shows which deployment is live and which is ready to go.
2. Select **Deployments** in the menu. A list of current and previous deployments is shown.
3. Find the previous deployment you want to roll back to and select **Make live** in the three dots menu. This will override the current live deployment.
