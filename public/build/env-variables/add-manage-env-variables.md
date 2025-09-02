
Environment variables are key-value pairs configured outside your source code so that each value can change depending on the [Environment](/build/environments/environments/). Common use cases are for API keys, and URLs or IDs which change per environment, as they can be more easily updated wherever the key is referenced. You can store as many environment variables in Kinde as you want. 


## Recommendations for variables

- Choose a key name that helps you easily recognize what the variable is for.
- Use descriptive, purpose-indicating names (e.g., `DATABASE_URL`, `API_KEY_STRIPE`).
- Use a consistent case, such as CamelCase, snake_case, kebab-case, etc. We recommend using UPPER_SNAKE_CASE for environment variables as this is a widely adopted convention.
- Mark a variable as sensitive if the value should be kept secret. For example an API key or password.
- Consider adding a prefix for different environments (e.g., `PROD_`, `DEV_`).
- Document the purpose and format of each variable for team reference.

## Add an environment variable

1. Go to **Settings > Data management > Env variables**.
2. Select **Add environment variable**.
3. In the dialog that opens, enter the **Key** and the **Value**.
4. Select if the key is **Sensitive**. 
5. Select **Save**.

## Update an environment variable value

You can only update the value of non-sensitive variables. If you need to update a sensitive variable value, you’ll need to delete and then create a new variable. 

1. Go to **Settings > Data management > Env variables**. A list of all your variables is shown.
2. Select **Edit variable** in the … three dots menu next to the relevant non-sensitive variable. The **Edit variable** dialog opens.  
3. Change the **Value**. You cannot change the `key`.
4. Select **Save**.

## Delete an environment variable

1. Go to **Settings > Data management > Env variables**. A list of all your variables is shown.
2. Select **Delete variable** in the … three dots menu next to the relevant variable. A confirmation window appears. 
3. Confirm the variable deletion.
