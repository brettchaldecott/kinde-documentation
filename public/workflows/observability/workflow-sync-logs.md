
A log entry is created every time your code is synced to Kinde from your repo. This log entry contains information about the sync process, including any errors or warnings that occurred during the sync.

## View code sync log

When your code hits Kinde from a fresh commit in your repo, we check to make sure we’re able to run it. If these checks fail, this will show up in the sync logs.

1. Go to **Settings > Git repo > Sync logs**. This shows a page list of all the sync events in chronological order.
2. Select the leftmost column or open the three dots menu next to an item in the list to view details of the sync. The **Sync event details** page opens.
3. Details of which processes succeeded and fails are shown.

## Common issues

### No workflows

At the point of syncing, Kinde checks to see if there are any workflows in your repo. If there are none, the sync will fail. You need to create a workflow before you can sync your code.

If there is a workflow file in your repo there are other reasons why Kinde may not be able to find it. Check the following:

- The workflow file is in the correct location. The workflow file must be in the root of your repo.
- The workflow file is a valid file type - i.e a `.ts` or `.js` file
- The file name finishes with `Workflow` - for example `m2mWorkflow.ts`.
- Your `kinde.json` file is pointing at the correct `kindeSrc`

### Unknown trigger requested

If you have a trigger in your workflow that Kinde does not recognize, the sync will fail. Check the following:

- The trigger is a valid trigger type. See the [about workflows](/workflows/about-workflows/) page for a list of valid triggers.
- The trigger is spelled correctly. Check for typos in the trigger name.

### More than one workflow assigned to this trigger

A trigger can only be assigned to one workflow. If you have more than one workflow assigned to the same trigger, the sync will fail. Check the following:

- The trigger is only assigned to one workflow. If you have more than one workflow assigned to the same trigger, remove the duplicate workflows.

### Duplicate workflow ID

A workflow ID must be unique. If you have more than one workflow with the same ID, the sync will fail. Check the following:

- The workflow ID is unique. If you have more than one workflow with the same ID, change the ID of one of the workflows.

### Missing output bundle

When the Kinde compiler runs, it creates a bundle of the workflow code. If this generated bundle is empty, the sync will fail.
