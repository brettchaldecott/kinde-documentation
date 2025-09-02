
<Aside type="upgrade">

You may need to upgrade your plan to use this feature

</Aside>

If you are on the Kinde Scale plan, you can change Kinde authenticated session configuration at the organization level as well as the environment level. An authenticated session (or SSO session) is the time during which a user is authenticated via Kinde, regardless of their activity. You can define if a session persists even after a browser is closed, and how long can lapse before making the organization's user re-authenticate.

These settings only apply to Kinde sessions and not sessions you maintain through your own application.

## Limitations of Kinde session configuration

- Session cookies are not destroyed when a tab is closed, the full browser window must be closed.
- Modern browsers usually allow session restoration. Restoring a browser session can also restore a session cookie.

## Manage SSO session behaviors and policies per organization

When you change session settings at the organization level, this overrides session settings at the environment level.

1. In Kinde, go to **Organizations** and open the organization whose session settings you want to configure.
2. Select **Sessions** in the side menu.
3. In the **SSO sessions** section, decide on the policy for session cookies. A persistent session leaves the cookie active when the browser is closed. A non-persistent session is terminated when the browser window closes (unless the limitations listed above apply).
4. In the **Session inactivity timeout** section, set how long a session can be inactive before prompting re-authentication. This setting is applied in seconds - where 3,600 seconds is one hour; 86,400 seconds is one day.
5. When you're finished, select **Save**.

The session settings will now be applied to members of this organization.

## Manage organization session behavior via API

Use this endpoint to update session settings [via API](https://docs.kinde.com/kinde-apis/management/#tag/organizations/patch/api/v1/organizations/{org_code}/sessions/). `PATCH /api/v1/organizations/{org_code}/sessions`
