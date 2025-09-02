
Kinde identifies workflows by searching for files named with the pattern `*Workflow.[ts/js]`. For instance, a workflow triggered upon m2m token generation might be named `m2mTokenWorkflow.ts`.

## Creating a workflow

To create a workflow, add a file named Workflow.[ts/js] within the `workflows` directory.

Each workflow file must export:

- A workflowSettings object defining the workflow's configuration.
- A default asynchronous function containing the workflow's logic.

The simplest workflow example:

```js
export const workflowSettings = {
  id: "onTokenGeneration",
  trigger: "user:tokens_generation"
};

export default async function Workflow({request, context}) {
  console.log("Hello world");
}
```

In this example, every time a user token is generated, this workflow will execute, logging "Hello world" to the console.

## API reference

The default exported function receives a single event argument, which is an object containing two keys: `context` and `request`.

### `request` (object)

Provides information about the request that triggered the workflow, such as the originating IP address and user agent string.

### `context` (object)

Contains additional, trigger-specific data relevant to the workflow, which can include details like the user ID or organization code.

Refer to the documentation for each specific workflow trigger to understand the data available within the `request` and `context` objects.
