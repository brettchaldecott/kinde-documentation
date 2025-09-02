
When you import users in bulk from a file, Kinde checks to see if there are errors that will prevent a new user record from being created or updated.

Common errors include:

- Duplicate records (based on the ID or email)
- Invalid email
- Missing or invalid information
- An organization, role, or permission does not exist in Kinde.

You can view the error log immediately after you import to review any errors.

## **To view errors**

After the import, you have the option to view the error list. This will tell you which items are in error.

1. In the spreadsheet, find the error and correct it.
2. Re-import the file.
3. If you cannot correct an error, you may need to [add the user manually](/manage-users/add-and-edit/add-and-edit-users/), which will trigger a password reset the next time they sign in.

If you find any errors you cannot resolve, [contact our support team](mailto:support@kinde.com).

## Fixing **errors**

The best way to fix errors is to correct them in the CSV file and then [import the users](/manage-users/add-and-edit/import-users-in-bulk/) again.

- You can do this as many times as you need to.
- You don’t need to delete records that have already been imported.
- Kinde ignores any information that exists already, unless you made any changes to it.

### Password errors

When importing JSON passwords, Kinde will look for a user matching the ID or email address in the user details file. We will let you know if a match is not found, if the hashing algorithm is not supported, or related columns are incorrect.

User’s whose passwords cannot be imported will be prompted to reset their password once you switch on the connection.

### Bad data from previous provider

If there are a lot of errors, you may need to go back to your old auth provider to get a new file.

### I**mporting users with organization IDs, roles, or permissions**

Organizations, roles, and permissions need to exist in Kinde before you bring these details in as part of a user import.

If an organization match is not found in Kinde, the user will be added to the default organization. To correct this, you may need to [add them to others manually,](/manage-users/about/manage-users-across-organizations/) or re-import.

Similarly, if the role or permission key does not exist in Kinde _exactly_ the way you referenced in the CSV, then the user might not be assigned the role or permission.
