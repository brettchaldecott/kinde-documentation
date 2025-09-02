
There are several logs that can be checked when something goes wrong.

## Sync log

Check to see if your code is syncing to Kinde and is able to be deployed. Issues with sync logs are most commonly associated with missing files or other directory issues.

Go to **Settings > Git repo > Sync log**.

## Build log

A build log is generated for each deployment. It provides details on the deployment checks, showing a success or fail message.

1. Go to **Design > Custom code**.
2. Select **Deployments** in the menu.
3. Select the specific deployment you want to view.
4. Scroll to the **Build log** section. For each page included in the deployment, open the dropdown to view the build details.

We recommend fixing all the errors detected during this process and syncing your code again. This generates a new deployment and new code check.

## Runtime log

Runtime logs record events that occur when your code is running. When something goes wrong, you can check runtime logs to see what happened.

1. Go to **Design > Custom code**.
2. Select **Runtime logs** in the menu.
3. Select the runtime ID to open a details page.
4. Scan the logs for errors.
