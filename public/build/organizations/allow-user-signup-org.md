
Kinde lets you easily manage which organizations users can sign up to.

For example, you can:

- allow or disable users from signing up to specific organizations (if you manage multiple organizations)
- allow or disable automatic sign up to the default organization (all Kinde businesses have a default)
- disable signups completely, and add users manually, via API, or by import.
- disable organizations from being created when a user signs up - this is usually only needed by businesses who’s customers are also businesses (B2B).

Follow the relevant procedure below to set up your preferences.

## Allow users to register to a specific organization

Each organization you set up in Kinde will have a unique `org_code`.

If you want users to sign up to a specific organization, the `org_code` must be passed with their registration request. You need to switch this functionality on per organization.

<Aside type="warning" title="Caution">

The `org_code` in the http request is vulnerable to manipulation, so only allow registrations to organizations that you would allow anyone to sign up to. If this is not the case, we suggest you leave this switched off and add the user to the right organization later via the Kinde management API.

</Aside>

1. Go to **Organizations** and select the organization.
2. On the **Policies** page, switch on the option to **Allow registrations**.
3. Select **Save**. Anyone can now register onto this organization if the `org_code` is passed with the registration request.

## Sign users up to the default organization

If the organization being signed up to is not known when a user signs up, the default organization is used. This applies to users created via API and by import as well.

If you want, you can switch this off to prevent sign-ups to the default org.

<Aside type="warning">

Users must belong to an organization to be assigned roles and permissions

</Aside>

1. Go to **Organizations** and open the default organization.
2. Go to **Policies**, then switch off the **Sign users up to the default organization if one can’t be detected** option.
3. Select **Save**.

## Disable user self-sign up

You can disable user self-sign up altogether. You might do this if you want to add and manage users manually, by API, or by import only.

1. Go to **Settings > Environment > Policies**.
2. Switch off the **Allow users to sign up** option.
3. Select **Save**. This setting applies to all apps in your business.

<Aside type="warning">

Leave this on if you plan to use [JIT provisioning for enterprise connections](/authenticate/enterprise-connections/provision-users-enterprise/).

</Aside>

## Sign in experience for users in multiple organizations

If a user belongs to multiple organizations, they will be prompted to select an organization when they sign in unless they pass a specific `org_code`.

If you use enterprise authentication, users will be recognized by their email domain and will be forwarded to the correct identity provider.
