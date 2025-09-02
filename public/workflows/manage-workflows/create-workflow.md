
You can create workflows using Kinde's provided triggers. All you need to do is add workflow code to your repo, that can be executed when the trigger fires.

Note that workflows only show up in Kinde if you have connected your repo and there are valid workflow files found in the branch you select. Otherwise, you won't see them.

## Add a workflow file to your repo

1. In the Kinde infrastructure folder, you set up (e.g. `[RootFolder]/environment/workflows`) create a TypeScript file. You can name it anything ending in `...Workflow.ts`. 
    
    For example if the Kinde workflow name is `Custom-access-token`, the filename could be `CustomAccessTokenWorkflow.ts.`
    
2. Enter your code into the file in plain JavaScript or TypeScript. For details on what must be included, see the [kinde/infrastructure readme file in Github](https://github.com/kinde-oss/infrastructure).
3. When you commit code in your repo and then sync the code in Kinde, we will perform validity tests on the files. If the commit is valid, it will have the status of ‘Draft’ in Kinde. You can make draft workflows live. 

## Make the workflow live (for the first time)

1. Got to **Workflows** and click on the workflow trigger. 
2. In the **Summary** screen for the workflow, find the draft deployment.
3. Select the three dots (…) menu on the far right and select **Make live**. 
4. Confirm you want to make the deployment live. 

For information on managing deployments and making a different deployment live, see the [workflows management](/workflows/manage-workflows/manage-code-and-deployments/) topic.
