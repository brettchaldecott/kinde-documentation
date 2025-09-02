
It’s easy to manage and control user access with permissions. Once you set up permissions, they can be grouped into [roles](/manage-users/roles-and-permissions/user-roles/), to make assigning them managing access easier.

## **First time creating permissions?**

For each permission you create on Kinde, you need to assign a unique ‘key’ that your product code will reference to apply the permission. We suggest you create permissions first, and then if you want, create roles to group sets of permissions to apply to users.

## **Add a new user permission**

1. Go to **Settings** **> User Management >** **Permissions**. If you already have permissions added, you’ll see a list of them.
2. Select **Add permission**.
3. Give the permission a **Name**. Keep it short and descriptive, so you can easily understand what it is for. For example, ‘View financial reports’.
4. Enter a **Description**. Provide additional context to help users understand this permission and the effect it will have. For example ‘Allows users to view, but not update, financial reports for the business.’
5. Enter a **Key**. The key is how your code references the permission in Kinde. It should be a word that is easy to reference in code and match in your product. For example `read-reports`. It’s a good idea to follow a naming convention pattern to help maintain your code. Here’s what it might look like:

   `[action_type]:[functional_area]` e.g `read:reports`

6. Select if you want this permission to be automatically added when a new role is created. You might do this for example, if the permission is something all users need to be allowed to do.
7. Select **Save**.

## **Edit permission**

User permissions are dynamic and refreshed via the issued token. This means that any changes you make will be applied to users, the next time they sign in.

We don’t recommend editing permission keys, once a permission is in use. It will break the code link between your product and the defined permission.

1. Go to **Settings** **> User Management > Permissions**. If you already have permissions, you’ll see a list of them.
2. Select the three dots next to the permission you want to edit and choose **Edit permission**.
3. Make the changes you want and select **Save**.

## Delete user permissions

When you delete a permission, you remove the permission access from all users who are assigned that permission, and from all users who have that permission as part of a role. This can’t be reversed.

1. Go to **Settings > User Management > Permissions**. Your list of permissions is shown.
2. Select the three dots next to the permission and choose **Delete**. A confirmation / warning message appears.
3. Select **Delete permission**. The permission is permanently deleted.
