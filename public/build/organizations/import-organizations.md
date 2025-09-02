
All Kinde businesses come with one default organization. For B2B business models, multiple organizations are usually required. [Learn more about organizations](/build/organizations/multi-tenancy-using-organizations/).

Use this procedure to import multiple organizations into Kinde via CSV, before you [import users](/manage-users/add-and-edit/import-users-in-bulk/) into those organizations.

You can also [add and manage organizations](/build/organizations/add-and-manage-organizations/) manually.

## Set up your CSV file

Before importing, you need to set up the CSV file with the organization details:

- Organization name
- External organization ID

The CSV should be set up like this:

```text
name, id
alpha, a001
beta, b002
charlie, c003
```

## Import organizations

You can use this procedure to add new organizations or update the details of your existing organizations.

1. In Kinde, go to **Organizations** and then select **Import organizations**.
2. In the window that opens, select **Choose file** and select the CSV file.
3. Select **Import**. The **Organizations** window now shows the imported organizations or changes.

Next: [Import users in bulk](/manage-users/add-and-edit/import-users-in-bulk/) and include the **external organization id** to assign users to the imported organizations.
