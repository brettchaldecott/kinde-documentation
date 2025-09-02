
Pages are rendered on the server which means it is possible to make API calls before the page is delivered to the browser. The most common use case for this is calling the Kinde Management API to get additional information for the page.

<Aside type="warning">

Just because you can do something doesn’t mean you should. Every API call adds latency to page render times slowing down the experience for your end users.

Where possible, we have provided the data you require either via functions in the npm package, or via the event object. The main reason for using the API is for B2B businesses who require more data for the current org to display. Let us know if you find other use cases or you think other data would be useful to include.

</Aside>

## Before using the Kinde Mangement API for pages

1. [Create an M2M application](/developer-tools/kinde-api/connect-to-kinde-api/) within Kinde and grant it access to the Kinde Management API with the desired scopes.
2. Setup environment variables in Kinde to provide the client ID and secret.

   By default the `createKindeAPI` method initially looks up the following environment variables setup in Kinde settings to determine the application to use.

   - `KINDE_WF_M2M_CLIENT_ID`
   - `KINDE_WF_M2M_CLIENT_SECRET` - Ensure this is setup with sensitive flag enabled to prevent accidental sharing

   `createKindeAPI` can also accept an additional parameter which can contain `clientID/clientSecret` or `clientIDKey/clientSecretKey` which can define the environment variables to look up.

## Calling the Kinde Management API for pages

The example below calls the `environment` API and adds the logo to the page.

**Required bindings**

```js
export const pageSettings = {
  bindings: {
    "kinde.env": {},
    "kinde.fetch": {},
    url: {}
  }
};
```

Example usage calling Organization API:

```jsx

export default async Page(event: onPageRequestEvent) {
  const orgCode = event.request.authUrlParams.orgCode;

  const kindeAPI = await createKindeAPI(event);

 const { data: res } = await kindeAPI.get({
    endpoint: `organization?code=${orgCode}`,
  });
  const {org} = res;

  return
    `<html
      lang={event.request.locale.lang}
    >
		    <head>
			    <title>Hello world</title>
		    </head>
		    <body>
			    <p>{org.name}</p>
		    </body>
	    </html>
	  `;
}
```

## Common API calls for custom page design

- `GET /environment` - includes brand settings for the environment including logos / background images. Required M2M scope: `read:environments`
- `GET /organizations?org_code=${request.authUrlParams.orgCode}` brand settings for the requested org including logo. Required M2M scope: `read:organizations`
- `GET /business` - business details like the business name. Required M2M scope: `read:businesses`
- `GET /applications/${request.authUrlParams.clientId}`. Required M2M scope: `read:applications`
