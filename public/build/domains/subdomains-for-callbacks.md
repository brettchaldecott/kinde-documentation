
If you use [organizations in Kinde](/build/organizations/multi-tenancy-using-organizations/), you probably build and maintain multi-tenant software. And if you manage multiple subdomains for those tenants, you'll need to make sure they are redirected to the right subdomain when they authenticate. The most effective way to do this is via ‘organization handles’ supported by a dynamic callback URL.

## How it works

For every organization that has its own subdomain, you will add an organization handle. The handle and the subdomain need to match exactly. Here's an example.

| Organization | Handle | Subdomain |
| --- | --- | --- |
| Red | red | red.everycolor.com |
| Green | green | green.everycolor.com |
| Blue | blue | blue.everycolor.com |

Then you add one dynamic callback URL to your application that routes all the users to the correct subdomain via the organization handle: `{organization.handle}.everycolor.com`.

The result is that users of the **Red** org will be automatically redirected to `red.everycolor.com` when they authenticate, and so on.

## Example authentication flow

![showing how handles and a dynamic callback route users during authentcation](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/e4e8e1ca-564f-4e25-602f-5f7cc51b1700/public)

## Requirements

- This feature works for top level subdomains only
- Handles must exactly match subdomain names
- Handles must be unique for each organization

## Add the dynamic callback URL to your application

1. In Kinde, go to **Settings > Applications > [your application] > Details.**
2. Scroll to the **Callback URLs** section and in the **Allowed callback URLs** field, add `https://{organization.handle}.<yourdomain>.com.`, and replace `<yourdomain>` with your primary domain. For example, `https://{organization.handle}.everycolor.com`
3. Select **Save**.
4. Repeat from step 1 for each app you want to enable this for.

## Add handles to your organizations

You can do this [via API](/kinde-apis/management#tag/organizations/patch/api/v1/organization/{org_code}) using the `handle` parameter, or manually in Kinde (see below).

1. In Kinde, go to **Organizations > [your organization] > Details.**
2. Enter the subdomain name for this organization in the **Handle** field, without the added URL information. For example, enter `blue` for the subdomain `https://blue.everycolor.com`. Ensure the subdomain name and the handle match exactly. The handle must also be unique within your Kinde business.
3. Select **Save**.
