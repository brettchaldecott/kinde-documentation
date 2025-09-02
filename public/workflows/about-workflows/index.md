
In Kinde, workflows exist to give you (almost) unlimited power over how Kinde works for you.

Develop code in your own repo, and push it to a Kinde workflow, where we execute it in ways that you define, triggered by Kinde events.

See the [Triggers](/workflows/example-workflows/workflow-user-post-auth/) section for the whole list of triggers.

We will add more (and allow you to create your own) as we develop the feature.

## When to use a workflow

- You want Kinde to do something that it doesn’t currently do.
- You want to customize an access or ID tokens in ways that we don’t support out of the box. For example, you want to fetch data from an external system to include it as claims in your tokens.
- You want to reduce the size and weight of tokens, remove things that you don’t need.
- You want to change the data type of a claim. For example changing from an array to a space delimited string.

## Basic workflow rules

- Workflow actions need to be written in TypeScript or plain JavaScript
- Filenames must end in the word 'Workflow' for Kinde to recognize them as containing workflow code. For example, `MFAWorkflow.ts` or `PreauthorizationWorkflow.jsx`
- File extensions should be .ts, .js, .tsx or .jsx
- Each trigger can only have one workflow
- Only Kinde-defined triggers are available (for now)

## Explanation of methods and code helpers

We’ve provided a set of methods and examples in our [Infrastructure repo in GitHub](https://github.com/kinde-oss/infrastructure). The README contains a guide to creating the code that powers your workflows.
