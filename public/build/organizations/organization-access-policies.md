
<Aside type="upgrade">

This is an advanced feature that is only available on the [Kinde Scale plan](https://kinde.com/pricing/). You can apply policies for 5 orgs, and then charges apply.

</Aside>

You can set policies for an organization to control access to the organization. When you set policies for an organization, this overrides any [policies set at the environment level](/build/set-up-options/access-policies/).

This topic provides instructions for the following tasks:

- Allow anyone to join the organization
- Only allow users from specific domains to sign in to the organization (i.e. apply domain restrictions for an organization)
- When users from specific domains sign up, add them as members to this organization
- Assign specific roles to new members of an organization

You may need to activate some features as you go.

## Go to the Policies section in your organization

1. In Kinde, go to **Organizations**.
2. Browse or search for the organization.
3. In the list, select the organization to open the details page.
4. Select **Policies** in the menu.
5. Configure policies using the following procedures.

### Configure access

1. If you want users to be able to join this organization if they pass the correct `org_code`, select **Allow org members to be auto-added**.
2. (Only in the default organization) Choose if you want to **Add users to this organization if no organization is specified** on sign up.

### Configure domain restrictions

1. Select **Allow org members to be auto-added** (if not already selected). 
2. Select **Auto-add users from allowed domains**.
3. Enter the domains in the **Allowed domains** list. Use the format `domain.com` and not `https://www.domain.com`. This restricts users from joining the organization unless they belong to an allowed domain. If you leave this empty, users from any domain can sign up.
4. Select **Save**.

### Configure default roles for new members

1. In the **Default roles** section, select which roles will be assigned to new members when they sign up.
![Select default roles](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/6fc3e960-8bfe-427e-1e34-051b515aa600/public)
2. Select **Save**.

## Policy setting quick reference

Note that your selections for each organization override the [global policy settings](/build/set-up-options/access-policies/).

| To…                                                                                                       | Do this…                                                                                                                                           |
| --------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| Allow anyone to join this organization.                                                             | Select **Allow org members to be auto-added**.                                                                                                                     |
| Sign up users who are not already associated with an organization, to the default organization.           | Select **Add users to this organization if no organization is specified**.                                                                         |
| Allow only people from `specificdomain.com` to sign up to this organization.                              | Select **Allow self sign-up** and enter the `specificdomain.com` into the **Allowed domains** list.                                                |
| Allow only people from `specificdomain.com` to sign up to this organization and auto-add them seamlessly. | Select **Allow self sign-up** and enter the `specificdomain.com` into the **Allowed domains** list and select Auto-add users from allowed domains. |
| Assign new members specific roles when they sign up                                                        | Select the **Default roles** you want assigned
