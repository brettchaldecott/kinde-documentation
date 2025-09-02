
We want you to keep control of your own code, in your own repo, so you can update and commit as usual. Then sync your code with one click and it’s ready to use.

You first need to set up your GitHub repo and connect the branch to Kinde.

## Set up your repo

For the code sync to work, your workflow files need to be stored in a specific place in your repo. This tells Kinde where your custom code lives. If you prefer to use an alternative directory you can change `kindeSrc` to something else.

1. In the root of the repo create a file called `kinde.json` with the content:

   ```jsx
   {
     "rootDir": "kindeSrc" // This is root folder Kinde will use to discover workflows.
   }
   ```

2. Create this folder structure in your repo's root directory: `[RootFolder]/environment/workflows`. This is where you will add the files that Kinde will read.

## Connect or change your repo and branch

1. In Kinde, go to **Settings > Environment > Git repo**.
2. Select **Connect repo** (first time) or **Change repo**.
3. Follow the on-screen prompts to connect your git service.
4. Select the repo and select **Next**.
5. Select the branch and select **Save**. You will know the connection is successful when you see the connected repo panel at the top of the screen.
6. If you are on an eligible Kinde plan, switch on the code preview mode option in **Advanced settings**. This lets you preview code in prod before making it live.
7. Check if your code has synced to Kinde. Go to **Home > Workflows**. If you already have workflow files set up, the workflows will appear in **Workflows** page. If we failed to read your workflow code, you should see a warning.

   ![workflow in Kinde](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/7f6299c4-5af2-406a-4d8b-7e6812a94e00/public)

8. Review any issues in the [Sync log](/workflows/testing/preview-workflows/).
9. You can re-sync the code at anytime by selecting **Sync code**.

<Aside type="warning">

If the preview code option is switched off, your code will be pushed live automatcially each time you sync. We recommend switching this off for production. This is a paid feature.

</Aside>

## Code status alerts

The home page of your Kinde dashboard shows a code alert status. You can see immediately if there are any concerns with your code. Select **View code status** to see if the issue is:

- a code sync problem
- a workflow code problem
- a design custom code issue
