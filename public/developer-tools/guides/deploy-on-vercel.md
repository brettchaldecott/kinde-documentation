
Vercel is a cloud platform which enables seamless deployment directly from a GitHub repository, offering scalability, performance, and security. You can use it to streamline the development and deployment process of applications. 

If you want to test what your app will look like with Kinde auth, follow this guide. Let’s deploy an app on Vercel!

## What you need

- A [Vercel](https://vercel.com/) account - You can sign up free using GitHub or your email.
- A [GitHub](https://github.com/) account - You can use third-party Git sources, but this tutorial will focus on GitHub.
- A deployed application that uses Kinde OR a starter kit such as the [Next.js starter kit](https://github.com/kinde-starter-kits/kinde-nextjs-app-router-starter-kit).

## (Optional) Set up a project using a Kinde starter kit

If you don’t have an existing project, create a project with a starter kit from Kinde.

1. Go to the [Kinde Starter Kits Github repository](https://github.com/kinde-starter-kits/) and find the starter kit you want to use.
2. Copy the starter kit by selecting **Use this template**, then select **Create a new repository**. This copies the kit content to your Github account. (You can also clone the repo to your GitHub account if you want.)
3. Enter a name for the new repository, then select **Create repository.**
4. Open the directory of the new repo and find the file called **`.env.local.sample`**. You will need this later.

## Step 1: Add a project in Vercel

1. Sign in to the Vercel [Dashboard](https://vercel.com/dashboard). You’ll see a list of all your projects if you have them. 
2. In the top right, select **Add New**, then select **Project**. 
    
   ![Shows new project screen in Vercel](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/6714b3d9-5725-454a-2dde-e06e6a832200/public)
    
3. Pick the repository you want to deploy to Vercel (e.g. the one you created above if you used the starter kit). This tutorial uses the project cloned from the previous step `vercel-nextjs-kinde`.
4. Select **Import** next to the project you want to deploy. 
5. On the **Configure Project** page, expand the **Environment Variables** section:
    
   ![Environment variables in Vercel](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/b95cb15a-4f2a-4aa7-9362-51561ea1c100/public)
    
6. Open the local `.env` file from your project and copy all the contents.
7. Paste the details into the **Environment Variables** section in Vercel. Vercel will autofill your keys and values for you.
    
   ![Paste environment variables in Vercel](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/faa5d8a5-7d00-4db8-d477-ba6607ad6e00/public)
    
8. Once you are done, select **Deploy**. Your project will deploy to Vercel. This may take a minute or two. When the process is finished, you will see the **Congratulations!** page.
9. Vercel generates a public URL for accessing your site. E.g. `https://vercel-nextjs-kinde.vercel.app/`. Copy the URL to use in a later step, where we will update the Kinde callback URLs.

## Step 2: Set up your application in Kinde

1. Sign in to your [Kinde](kinde.com) business and go to **Settings.**
2. Select **Applications.**
3. If you do not have any applications yet, create one. [Follow this guide](https://docs.kinde.com/build/applications/add-and-manage-applications/).
4. If you have already created your app, select **View details** on the application tile. 
5. Scroll to the **App keys** section and copy the **Domain, Client ID, and Client secret** somewhere you can access it again.
    
   ![App keys section in your Kinde app](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/f6b6879b-2e12-4255-6b7d-864b57c3a400/public)
    
6. Scroll to the **Callback URLs** section and enter the URL you copied when you deployed on Vercel. The URL will be something like `<NAME_OF_YOUR_PROJECT>.vercel.app`, for example `https://vercel-nextjs-kinde.vercel.app`. 
7. Enter the URL in the **Application homepage URI** field, the **Application login URI** field, and the **Allowed logout redirect URLs** field.
8. If you were using `https://localhost:3000`, replace it with your new domain in the **Allowed callback URLs** field. If you're using a starter kit, this might be: `https://vercel-nextjs-kinde.vercel.app/dashboard`. 
    
   ![Updating Kinde callbacks](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/e4351ab5-a0cc-446c-f124-8f941c855900/public)
    
9. Select **Save.**

## Step 3: Update Vercel environment variables

1. Open your [Vercel project dashboard](https://vercel.com/dashboard) and navigate to your project. Select the three dots … menu, then **Settings**.
2. Select **Environment Variables**.
3. Edit the values of each environment variable by selecting three dots menu, then selecting **Edit**.
    
   ![Update Vercel variables again](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/339ec567-e386-4393-b4bd-da0f5b34fe00/public)
    
4. In the window that opens, enter the new value for the variable, then select **Save**.
5. Change the variables as follows, using Kinde values:
    - KINDE_CLIENT_ID
    - KINDE_CLIENT_SECRET
    - KINDE_ISSUER_URL (Domain in Kinde)
6. Change the variables as follows, using Vercel values (e.g. `https://vercel-nextjs-kinde.vercel.app`):
    - KINDE_SITE_URL
    - KINDE_POST_LOGOUT_REDIRECT_URL
    - KINDE_POST_LOGIN_REDIRECT_URL
    
    You need to deploy your Vercel instance to apply the updated variables. 
    
7. Go to the **Deployments** section.
8. Select the three dots … menu and select **Re-deploy**.
    
    <Aside>
      
    Do not select **Use existing Build Cache** as that will preserve the old environment variables
    
    </Aside>
    
    Your website will now be fully functional and you can authenticate with Kinde! 
    
    ![Shows Kinde verification page for auth](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/d7ecbc5a-08ee-407c-f2a6-60e753198200/public)

Remember, if you need any assistance with getting Kinde connected reach out to [support@kinde.com](mailto:support@kinde.com).
You can also join the [Kinde community on Slack](https://join.slack.com/t/thekindecommunity/shared_invite/zt-26hdaavyc-CfOa06vP23guSwK~~OpFMQ) or the [Kinde community on Discord](https://discord.com/invite/tw5ng5tK6V) for support and advice from the team and others working with Kinde.
    
Congratulations 🎉 on deploying your Kinde project to Vercel!
