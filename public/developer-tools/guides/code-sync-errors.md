
If you’ve synced your repository to pass code to Kinde for custom pages or workflows, you may encounter errors from time to time. Here are some common issues and how to resolve them. 

## Deployment failed: More than one workflow assigned to this trigger

Each trigger can be mapped to only one workflow. Make sure you don’t already have a workflow using the same trigger.

## No default function exported

Kinde expects a default function to be exported. Make sure the format is correct.

```jsx
// Pages
export default async function Page(event) {

  return "<div>Page content</div>";
}

// Workflows
export default async function Workflow(event) {

  // Workflow code here
}
```

## Workflow ID is required

Workflows require a unique ID to be set in the `workflowSettings` object.

If an ID is present in your code, this usually means the page failed to compile and the settings could not be read.

## Sync log showing no pages and no workflows

Technically the sync was successful but Kinde was unable to find any pages or workflows in your repo. Check your `kindeSrc` file and default exports.

## No matching export in 

`"kindeKnownPackage:https://cdn.jsdelivr.net/npm/@kinde/infrastructure@latest/+esm" for import <xxxxx>`

Occurs if you are trying to use a method from the Kinde infrastructure package that doesn’t exist. Check your imports.

## Runtime error: `x` is not defined at `y`

Check your code for undefined variable usage.

