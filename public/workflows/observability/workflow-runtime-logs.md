
When your code is running in a workflow, we monitor performance and activity as the workflow executes. If you discover or someone reports an issue with the workflow, runtime logs are likely to pick up any errors.

## View runtime logs

1. Go to **Workflows**.
2. Select a workflow in the list to open it. The **Summary** page opens.
3. Select **Runtime logs** in the menu. A list of all your recent logs shows.
4. Select the runtime log you want to view.

## Common issues

The Kinde runtime environment is a secure JavaScript environment. This means that some JavaScript features are not available in the Kinde runtime. If you try to use a feature that is not supported, you will see an error in the runtime logs.

For example, if you try to use the `fs` module to read a file from the file system, you will see an error like this:

```
Error: fs is not defined
```
