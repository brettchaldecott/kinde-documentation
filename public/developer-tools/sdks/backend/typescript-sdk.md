
Kinde’s TypeScript SDK allows developers to integrate Kinde Authentication into their JavaScript or TypeScript projects. This SDK implements the following OAuth2.0 flows. [Learn more here](https://kinde.com/guides/authentication/protocols/oauth-flows-explained/)

- **Authorization Code** - Intended for confidential clients, e.g. web-servers
- **Authorization Code with PKCE extension** - For public clients, e.g. single page web application and or mobile applications
- **Client Credentials flow** - Intended for confidential clients, where machine to machine communication is required.

## Requirements

Node version 16 or newer

## Register for Kinde

If you haven’t already got a Kinde account, [register for free here](https://app.kinde.com/register) (no credit card required). Registering gives you a Kinde domain, which you need to get started, e.g. `<your_subdomain>.kinde.com`.

## Install

<PackageManagers pkg="@kinde-oss/kinde-typescript-sdk" />

## Configure Kinde

### Set callback URLs

Kinde will redirect your user to authenticate. They’ll be redirected back to your JavaScript app after signing in or signing up.

To authenticate your app, you need to specify which URL Kinde should redirect your user.

1. In Kinde, go to **Settings > Applications** and then navigate to **Front-end app** or **Back-end app** which ever applies - or add a new application.
2. Add your callback URLs in the relevant fields. For example:

   - Allowed callback URLs (also known as redirect URIs):
     `https://<your_app_domain>/callback` e.g: `http://localhost:3000/callback`
   - Allowed logout redirect URLs:

     `https://<your_app_domain>` e.g:`http://localhost:3000`

3. Select **Save**.

### Add environments

Kinde comes with a production environment, but you can set up other environments if you want to. Each environment has a unique subdomain so be sure to use the correct one in the **Configure your app section** which follows.

## Integrate with your app

First step is to configure and create a client. The following settings are needed depending on which authentication flow you are using.

You can find these values on your **Settings > Applications > [Your app] > View details** page.

- `authDomain` - your Kinde domain
- `clientId` - your Kinde client ID
- `clientSecret` - your Kinde client secret
- `redirectURL` - your callback url to redirect to after authentication. Make sure this URL is under your **Allowed callback URLs**.
- `logoutRedirectURL` - where you want users to be redirected to after logging out. Make sure this URL is under your **Allowed logout redirect URLs**.

```typescript
import {createKindeServerClient, GrantType} from "@kinde-oss/kinde-typescript-sdk";

// Client for authorization code flow
const kindeClient = createKindeServerClient(GrantType.AUTHORIZATION_CODE, {
  authDomain: "https://<your_kinde_subdomain>.kinde.com",
  clientId: "<your_kinde_client_id>",
  clientSecret: "<your_kinde_client_secret>",
  redirectURL: "http://localhost:3000/callback",
  logoutRedirectURL: "http://localhost:3000"
});

// Client for client credentials flow
const kindeApiClient = createKindeServerClient(GrantType.CLIENT_CREDENTIALS, {
  authDomain: "https://<your_kinde_subdomain>.kinde.com",
  clientId: "<your_kinde_client_id>",
  clientSecret: "<your_kinde_client_secret>",
  logoutRedirectURL: "http://localhost:3000"
});
```

## Log in and register

To incorporate the login and register features, you'll need to redirect to Kinde for authentication. One way to do this is to create routes for `/login` and `/register`.

```typescript
const app = express();

app.get("/login", async (req, res) => {
  const loginUrl = await kindeClient.login(sessionManager);
  return res.redirect(loginUrl.toString());
});

app.get("/register", async (req, res) => {
  const registerUrl = await kindeClient.register(sessionManager);
  return res.redirect(registerUrl.toString());
});

app.listen(3000);
```

With that in place you can simply add links in your HTML as follows:

```html
<a href="/login">Sign in</a> <a href="/register">Sign up</a>
```

In the above example there is a `sessionManager` which has not been defined. In order to track the authenticated session between requests a session store is required. Any key-value store can be used for this, you just need to implement the `SessionManager` interface to provide it to the SDK.

An example session manager storing in memory could be implemented as:

```typescript
let store: Record<string, unknown> = {};

const sessionManager: SessionManager = {
  async getSessionItem(key: string) {
    return store[key];
  },
  async setSessionItem(key: string, value: unknown) {
    store[key] = value;
  },
  async removeSessionItem(key: string) {
    delete store[key];
  },
  async destroySession() {
    store = {};
  }
};
```

This would work for a single user for local development purposes, but would need to be expanded for a production environment.

The appropriate session store for your application will depend on your application architecture, for example encrypted cookies in a stateless server environment or a shared cache/database for a load balanced cluster of servers.

Commonly, the session manager will be a wrapper around an existing session management library - often provided by a web framework, or a third party library.

## **Manage redirects**

You will also need to route `/callback`. When the user is redirected back to your site from Kinde, it will trigger a call to the callback URL defined in the `redirectURL` client option.

```typescript
app.get("/callback", async (req, res) => {
  const url = new URL(`${req.protocol}://${req.get("host")}${req.url}`);
  await kindeClient.handleRedirectToApp(sessionManager, url);
  return res.redirect("/");
});
```

## Logout

The Kinde SDK comes with a logout method.

```typescript
app.get("/logout", async (req, res) => {
  const logoutUrl = await kindeClient.logout(sessionManager);
  return res.redirect(logoutUrl.toString());
});
```

```html
<a href="/logout">Log out</a>
```

## Check if user is authenticated

We’ve provided a helper to get a boolean value to check if a user is signed in by verifying that the access token is still valid. The `isAuthenticated` function is only available for authentication code and PKCE flows.

```typescript
const isAuthenticated = await kindeClient.isAuthenticated(sessionManager); // Boolean: true or false
if (isAuthenticated) {
  // Need to implement, e.g: call an api, etc...
} else {
  // Need to implement, e.g: redirect user to sign in, etc..
}
```

## View user profile

You need to have already authenticated before you call the API, otherwise an error will occur.

To access the user information, use the `getUserProfile` helper function:

```typescript
const profile = await kindeClient.getUserProfile(sessionManager);
// returns
{
  "given_name":"Dave",
	"id":"kp_12345678910",
	"family_name":"Smith",
	"email":"dave@smith.com",
	"picture": "https://link_to_avatar_url.kinde.com"
}
```

## **View users in Kinde**

Go to the **Users** page in Kinde to see your newly registered user.

## Audience

An `audience` is the intended recipient of an access token - for example the API for your application. The audience argument can be passed to the Kinde client to request an audience be added to the provided token.

```typescript
const clientOptions = {
	...,
  audience: 'api.yourapp.com',
};

const kindeClient = createKindeServerClient(
  GrantType.AUTHORIZATION_CODE, clientOptions
);
```

## Overriding scope

By default the Kinde SDK requests the following scopes:

- `profile`
- `email`
- `offline`
- `openid`

You can override this by passing scopes into the Kinde SDK

```typescript
const clientOptions = {
  ...,
  scope: 'openid profile email offline',
};

const kindeClient = createKindeServerClient(
  GrantType.AUTHORIZATION_CODE, clientOptions
);
```

## Organizations

### Create an organization

To have a new organization created within your application during registration, you can create a route as follows:

```typescript
app.get("/createOrg", async (req, res) => {
  const org_name = req.query.org_name?.toString();
  const createUrl = await kindeClient.createOrg(sessionManager, {org_name});
  return res.redirect(createUrl.toString());
});
```

You can also pass `org_name` as part of the query string as per the following HTML:

```html
<a href="/createOrg?org_name=<your_org_name>">Create Org</a>
```

### Log in and register to organizations

The Kinde client provides methods for you to easily log in and register users into existing organizations.

Update the routes to accept an `org_code` parameter and pass it to the SDK:

```typescript
const loginUrl = await kindeClient.login(sessionManager, {org_code: "org_1234"});

const registerUrl = await kindeClient.register(sessionManager, {org_code: "org_1234"});
```

Following authentication, Kinde provides a JSON web token (JWT) to your application. Along with the standard information we also include the `org_code` and the permissions for that organization (this is important as a user can belong to multiple organizations and have different permissions for each).

Example of a returned token:

```json
{
  "aud": [],
  "exp": 1658475930,
  "iat": 1658472329,
  "iss": "https://your_subdomain.kinde.com",
  "jti": "123457890",
  "org_code": "org_1234",
  "permissions": ["read:todos", "create:todos"],
  "scp": ["openid", "profile", "email", "offline"],
  "sub": "kp:123457890"
}
```

The `id_token` will also contain an array of organizations that a user belongs to - this is useful if you wanted to build out an organization switcher for example.

```json
[
		...
		"org_codes": ["org_1234", "org_4567"]
		...
];
```

There are two helper functions you can use to extract information:

```typescript
const org = await kindeClient.getOrganization(sessionManager); // { orgCode: 'org_1234' }

const orgs = await kindeClient.getUserOrganizations(sessionManager); // { orgCodes: ['org_1234', 'org_abcd'] }
```

## User permissions

Once a user has been verified, your product/application will return the JWT token with an array of permissions for that user. You will need to configure your product/application to read permissions and unlock the respective functions.

[Set permissions](/manage-users/roles-and-permissions/user-permissions/) in your Kinde account. Here’s an example set of permissions.

```typescript
const permissions = [
  "create:todos",
  "update:todos",
  "read:todos",
  "delete:todos",
  "create:tasks",
  "update:tasks",
  "read:tasks",
  "delete:tasks
];
```

We provide helper functions to more easily access the permissions claim:

```typescript
const permission = await kindeClient.getPermission(sessionManager, "create:todos"); // { orgCode: 'org_1234', isGranted: true }

const permissions = await kindeClient.getPermissions(sessionManager); // { orgCode: 'org_1234', permissions: ['create:todos', 'update:todos', 'read:todos'] }
```

A practical example in code might look something like:

```typescript
const permission = await kindeClient.getPermission(sessionManager, "create:todos");
if (permission.isGranted) {
    ...
}
```

## Getting claims

We have provided a helper to grab any claim from your id or access tokens. The helper defaults to access tokens:

```typescript
client.getClaim(sessionManager, "aud"); // { name: "aud", value: ["local-testing@kinde.com"] }

client.getClaimValue(sessionManager, "aud"); // ["local-testing@kinde.com"]

client.getClaim(sessionManager, "email", "id_token"); // { name: "email", value: "first.last@test.com" }

client.getClaimValue(sessionManager, "email", "id_token"); // "first.last@test.com
```

## Feature Flags

We have provided a helper to return any features flag from the access token:

```typescript
client.getFeatureFlag(sessionManager, 'theme')
// returns
{
	"is_default": false
  "value": "pink",
  "code": "theme",
  "type": "string",
}

client.getFeatureFlag(sessionManager, 'no-feature-flag')
// returns
// Error: "Flag no-feature-flag was not found, and no default value has been provided"

client.getFeatureFlag(sessionManager, 'no-feature-flag', 'default-value')
// returns
{
	"is_default": true
	"code": "no-feature-flag",
	"value": "default-value",
}

client.getFeatureFlag(sessionManager, 'theme', 'default-theme', 'b')
// returns
// Error: "Flag theme is of type string, expected type is boolean"
```

We also require wrapper functions by type which should leverage `getFlag` above.

### **Get boolean flags**

```typescript
/**
 * Get a boolean flag from the feature_flags claim of the access_token.
 * @param {Object} request - Request object
 * @param {String} code - The name of the flag.
 * @param {Boolean} [defaultValue] - A fallback value if the flag isn't found.
 * @return {Boolean}
 */
kindeClient.getBooleanFlag(sessionManager, code, defaultValue);

kindeClient.getBooleanFlag(sessionManager, "is_dark_mode");
// true

kindeClient.getBooleanFlag(sessionManager, "is_dark_mode", false);
// true

kindeClient.getBooleanFlag(sessionManager, "new_feature");
// Error - flag does not exist and no default provided

kindeClient.getBooleanFlag(sessionManager, "new_feature", false);
// false (flag does not exist so falls back to default)

kindeClient.getBooleanFlag(sessionManager, "theme", "blue");
// Error - Flag "theme" is of type string not boolean
```

### **Get string flags**

```typescript
/**
 * Get a string flag from the feature_flags claim of the access_token.
 * @param {Object} request - Request object
 * @param {String} code - The name of the flag.
 * @param {String} [defaultValue] - A fallback value if the flag isn't found.
 * @return {String}
 */
kindeClient.getStringFlag(sessionManager, code, defaultValue);

/* Example usage */
kindeClient.getStringFlag(sessionManager, "theme");
// pink

kindeClient.getStringFlag(sessionManager, "theme", "black");
// true

kindeClient.getStringFlag(sessionManager, "cta_color");
// Error - flag does not exist and no default provided

kindeClient.getStringFlag(sessionManager, "cta_color", "blue");
// blue (flag does not exist so falls back to default)

kindeClient.getStringFlag(sessionManager, "is_dark_mode", false);
// Error - Flag "is_dark_mode" is of type boolean not string
```

### **Get integer flags**

```typescript
/**
 * Get an integer flag from the feature_flags claim of the access_token.
 * @param {Object} request - Request object
 * @param {String} code - The name of the flag.
 * @param {Integer} [defaultValue] - A fallback value if the flag isn't found.
 * @return {Integer}
 */
kindeClient.getIntegerFlag(sessionManager, code, defaultValue);

kindeClient.getIntegerFlag(sessionManager, "competitions_limit");
// 5

kindeClient.getIntegerFlag(sessionManager, "competitions_limit", 3);
// 5

kindeClient.getIntegerFlag(sessionManager, "team_count");
// Error - flag does not exist and no default provided

kindeClient.getIntegerFlag(sessionManager, "team_count", 2);
// false (flag does not exist so falls back to default)

kindeClient.getIntegerFlag(sessionManager, "is_dark_mode", false);
// Error - Flag "is_dark_mode" is of type boolean not integer
```

## Token storage

After the user has successfully logged in, you will have a JSON Web Token (JWT) and a refresh token securely stored. You can retrieve an access token by utilizing the `getToken` method.

```typescript
const accessToken = await kindeClient.getToken(sessionManager);
```

## Kinde Management API

To use our management API please see [@kinde/management-api-js](https://github.com/kinde-oss/management-api-js)

## **SDK API reference**

### `authDomain`

Either your Kinde instance url or your custom domain. e.g. `https://yourapp.kinde.com`.

Type: `string`

Required: Yes

### `redirectUri`

The url that the user will be returned to after authentication.

Type: `string`

Required: Yes

### `LogoutRedirectUri`

The url that the user will be returned to after they sign out.

Type: `string`

Required: Yes

### `clientId`

The ID of your application in Kinde.

Type: `string`

Required: Yes

### `clientSecret`

The unique client secret associated with your application in Kinde.

Type: `string`

Required: Yes

### `scope`

The scopes to be requested from Kinde.

Type: `string`

Required: No

Default: `openid profile email offline`

### `audience`

The audience claim for the JWT.

Type: `string`

Required: No

## **Kinde SDK methods**

### `login`

Constructs a redirect URL and sends the user to Kinde to sign in.

Arguments:

```typescript
sessionManager: SessionManager;
```

Usage:

```typescript
kindeClient.login(sessionManager);
```

### `register`

Constructs a redirect URL and sends the user to Kinde to sign up.

Arguments:

```typescript
sessionManager: SessionManager;
```

Usage:

```typescript
kindeClient.register(sessionManager);
```

### `logout`

Logs the user out of Kinde.

Arguments:

```typescript
sessionManager: SessionManager;
```

Usage:

```typescript
kindeClient.logout(sessionManager);
```

### `handleRedirectToApp`

Callback middleware function for Kinde OAuth 2.0 flow.

Arguments:

```typescript
sessionManager: SessionManager;
callbackURL: URL;
```

Usage:

```typescript
kindeClient.handleRedirectToApp(sessionManager, callbackURL);
```

### `isAuthenticated`

Check if the user is authenticated.

Arguments:

```typescript
sessionManager: SessionManager;
```

Usage:

```typescript
await kindeClient.isAuthenticated(sessionManager);
```

Output: `true` or `false`

### `createOrg`

Constructs redirect url and sends user to Kinde to sign up and create a new org for your business.

Arguments:

```typescript
sessionManager : SessionManager
options?: CreateOrgURLOptions
{
   org_name?: string;
   org_code?: string;
   state?: string;
}
```

Usage:

```typescript
kindeClient.createOrg(sessionManager, {
  org_name: "org_1234"
});
```

### `getClaim`

Extract the provided claim from the provided token type in the current session, the returned object includes the provided claim.

Arguments:

```typescript
sessionManager : SessionManager
tokenKey?: ClaimTokenType 'access_token' | 'id_token’
```

Usage:

```typescript
kindeClient.getClaim(sessionManager, "given_name", "id_token");
```

### `getClaimValue`

Extract the provided claim from the provided token type in the current session.

Arguments:

```typescript
sessionManager : SessionManager
claim: string, 
tokenKey?: ClaimTokenType 'access_token' | 'id_token’
```

Usage:

```typescript
client.getClaimValue(sessionManager, "given_name");
```

Output: `'David'`

### `getPermission`

Returns the state of a given permission.

Arguments:

```typescript
sessionManager: SessionManager;
key: string;
```

Usage:

```typescript
kindeClient.getPermission(sessionManager, "read:todos");
```

Output sample:

```json
{
  "orgCode": "org_1234",
  "isGranted": true
}
```

### `getPermissions`

Returns all permissions for the current user for the organization they are logged into.

Arguments:

```typescript
sessionManager: SessionManager;
```

Usage:

```typescript
kindeClient.getPermissions(sessionManager);
```

Sample output:

```json
{
  "orgCode": "org_1234",
  "permissions": ["create:todos", "update:todos", "read:todos"]
}
```

### `getOrganization`

Get details for the organization your user is logged into.

Arguments:

```typescript
sessionManager: SessionManager;
key: string;
```

Usage:

```typescript
kindeClient.getOrganization(sessionManager);
```

Sample output:

```json
{"orgCode": "org_1234"}
```

### `getUserOrganizations`

Gets an array of all organizations the user has access to.

Arguments:

```typescript
sessionManager: SessionManager;
```

Usage:

```typescript
kindeClient.getUserOrganizations(sessionManager);
```

Sample output:

```json
{"orgCodes": ["org_7052552de", "org_5a5c293813"]}
```

### `getUser`

Extracts the user details from the ID token obtained post authentication.

Arguments:

```typescript
sessionManager: SessionManager;
```

Usage:

```typescript
kindeClient.getUser(sessionManager);
```

### `getToken`

Returns a valid access token if available.

Arguments:

```typescript
sessionManager: SessionManager;
```

Usage:

```typescript
kindeClient.getToken(sessionManager);
```

### `refreshTokens`

Uses the refresh token to update and return new tokens.

Arguments:

```typescript
sessionManager: SessionManager;
```

Usage:

```typescript
kindeClient.refreshTokens(sessionManager);
```

### `getUserProfile`

Extracts makes use of the `getToken` method above to fetch user details.

Arguments:

```typescript
sessionManager: SessionManager;
```

Usage:

```typescript
kindeClient.getUserProfile(sessionManager);
```

Sample output:

```json
{"given_name": "Dave", "id": "abcdef", "family_name": "Smith", "email": "mailto:dave@smith.com"}
```

### `getFlag`

Get a flag from the feature_flags claim of the `access_token`.

Arguments:

```typescript
sessionManager : SessionManager
code : string
defaultValue? :  FlagType[keyof FlagType
flagType? : keyof FlagType

interface FlagType {
    s: string;
    b: boolean;
    i: number;
}
interface GetFlagType {
    type?: 'string' | 'boolean' | 'number';
    value: FlagType[keyof FlagType];
    is_default: boolean;
    code: string;
}
```

Usage:

```typescript
kindeClient.getFlag(sessionManager, "theme");
```

Sample output:

```json
{
  "code": "theme",
  "type": "string",
  "value": "pink",
  "is_default": false
}
```

### `getBooleanFlag`

Get a boolean flag from the `feature_flags` claim of the access token.

Arguments:

```typescript
sessionManager : SessionManager
code : string
defaultValue? :  boolean
```

Usage:

```typescript
kindeClient.getBooleanFlag(sessionManager, "is_dark_mode");
```

Sample output: `true`

### `getStringFlag`

Get a string flag from the `feature_flags` claim of the access token.

Arguments:

```typescript
sessionManager : SessionManager
code : string
defaultValue? :  string
```

Usage:

```typescript
kindeClient.getStringFlag(sessionManager, "theme");
```

Sample output: `pink`

### `getIntegerFlag`

Get an integer flag from the `feature_flags` claim of the access token.

Arguments:

```typescript
sessionManager : SessionManager
code : string
defaultValue? :  number
```

Usage:

```typescript
kindeClient.getIntegerFlag(sessionManager, "team_count");
```

Sample output: `2`

If you need help connecting to Kinde, please contact us at [support@kinde.com](mailto:support@kinde.com).
