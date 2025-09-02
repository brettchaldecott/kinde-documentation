
A log entry is created every time your code is compiled and deployed on Kinde. This log entry contains information about the build process, including any errors or warnings that occurred during the build.

## View build logs

If your code was succesfully synced from your repo Kinde will then attempt to build (compile and deploy) your code. The results of this build will show up in the deployment build logs.

1. Go to **Workflows**.
2. Select a workflow in the list to open it. The **Summary** page opens.
3. Select **Deployments** in the menu. A list of all your deployments shows.
4. Select the deployment you want to test. The **Deployment details** page opens.
5. Scroll to the bottom of the page and view the build logs.

## Common issues

### Failed to resolve file, invalid location

Typically this means that the file you are trying to import is not in the correct location. Check the following:

- The file is in the correct location.
- The file you are importing is a valid file type - i.e a `.ts` or `.js` file
- If you are using a package from the NPM registry make sure it is a Kinde supported package.
