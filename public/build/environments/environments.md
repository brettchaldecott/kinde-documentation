
Kinde allows you to run multiple environments to support your software development cycle. All plans come with one production environment and one non-production environment. Higher plans come with additional non-production environments, so you can add staging, testing, beta, etc. to meet your team’s needs.

<Aside>

You must use the default production environment (the one you got when you first signed up to Kinde) as your production environment. This is important for when you are [ready to go live](/build/environments/production-to-live/). You cannot delete the production environment or add additional production environments.

</Aside>

You can see the environment you are working in at the top left of the home page when signed in.

## Take care using multiple environments

Unless you are a developer, we recommend you stay in the production environment for viewing and working with your Kinde account.

For developers who want to test new user configurations or make other changes, do this in a non-production (testing or staging) environment first, before replicating in the production environment.

## Rate limiting for live production environments

Before you make your production environment live, [follow ](/build/environments/production-to-live/)[this checklist](/build/environments/production-to-live/).

It is critical that all [third-party authentication](/authenticate/authentication-methods/set-up-user-authentication/) connections (such as social connections or enterprise connections) are set up with the third-party’s Client ID and Client secret. If you leave these fields blank during setup, Kinde’s credentials will be used as a proxy, and rate limits will apply.

Note that you can leave these fields blank in your non-production environments. Non-production environments cannot be made live.

## View and manage environments

Select the environment drop-down in the top left of the home screen. From here you can:
- Switch to another environment
- View all your environments (and create additional ones)
- View environment settings

## Add an environment

You can only add non-production environments.

1. Select the environment drop-down in the top left of the home screen, and select **All environments**.
2. On the **Environments** page, select **Add environment**. 
3. Enter a name and code for the environment, then select **Save**.

## Switch between environments

Select the environment drop-down in the top left of the home screen, and select the environment you want to switch to.

## Change the name of an environment

1. Switch to the environment you want. 
2. Select the environment drop-down in the top left of the home screen and choose **Environment settings**.
3. Change the environment name and select **Save**.

## Delete a non-production environment

You cannot delete the production environment.

1. Select the environment drop-down in the top left of the home screen, and select **All environments**.
2. On the environment you want to delete select the three dots, then select **Delete environment**.
3. Confirm you want to delete the environment. This action is irreversible.

## Move data between environments

There is currently no way to shift data or configuration settings between environments. For now, you need to replicate settings manually, and export/import data or use the [Kinde Management API](https://docs.kinde.com/kinde-apis/management/). It's on our roadmap to make this easier someday.
