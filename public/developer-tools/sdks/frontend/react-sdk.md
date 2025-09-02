
The Kinde React SDK allows developers to quickly and securely integrate a new or an existing React application to the Kinde platform.

You can also view the [React package](https://github.com/kinde-oss/kinde-auth-react) and [React starter kit](https://github.com/kinde-starter-kits/react-starter-kit) in GitHub.

This new SDK (v5) is optimized to work with React version 18+.

If you are currently using v4, refer to this [migration information](/developer-tools/sdks/frontend/react-sdk/#migration-from-v4-to-v5) to update to v5.

## **Register for Kinde**

If you haven’t already got a Kinde account, [register for free here](https://app.kinde.com/register) (no credit card required). Registering gives you a Kinde domain, which you need to get started, e.g. `yourapp.kinde.com`.

## **Configure React**

### Installation

<PackageManagers pkg=" @kinde-oss/kinde-auth-react" />

## **Integrate with your app**

Kinde uses a React Context Provider to maintain its internal state in your application.

Import the Kinde Provider component and wrap your application in it.

```jsx
import { KindeProvider } from '@kinde-oss/kinde-auth-react';
const App = () => (
  <KindeProvider
    clientId="<your_kinde_client_id>"
    domain="<your_kinde_domain>"
    logoutUri={window.location.origin}
    redirectUri={window.location.origin}
  >
    <Routes />
  </KindeProvider>
);
```

## **Set callback and logout URLs**

Set the URLs in Kinde so that after your user signs up, signs in, or signs out, they will be redirected back to your application.

1. In Kinde, go to **Settings > Applications > [your app] > View details**.
2. Replace the `your_kinde_client_id` and `your_kinde_domain` placeholders in the code block above with the the values from the **App keys** section.
3. Add your callback URLs in the relevant fields. For example:
   - Allowed callback URLs (also known as redirect URIs) - for example `https://localhost:3000/home/callback`
   - Allowed logout redirect URLs - for example `https://localhost:3000`
4. Select **Save**.

Tip: Make sure there are no hidden spaces and remove any ‘/’ forward slashes from the end of URLs.

## Environments

If you would like to use different Environments as part of your development process, you will need to [add them within your Kinde business](/build/environments/environments/) first. You will also need to add the Environment subdomain to the code block above.

## Sign in and sign up

Kinde provides a `useKindeAuth` hook with the methods `login` and `register` method to start the flow and also `LoginLink` and `RegisterLink` components

Use the button examples below to redirect your users to Kinde, where they authenticate before being redirected back to your site.

```jsx
import { LoginLink, RegisterLink } from '@kinde-oss/kinde-auth-react/components';

<LoginLink>Sign in</LoginLink>
<RegisterLink>Sign up</RegisterLink>
```

```jsx
import { useKindeAuth } from '@kinde-oss/kinde-auth-react';

const { login, register } = useKindeAuth();

<button onClick={() => register(/* params here */)} type="button">Sign up</button>
<button onClick={() => login(/* params here */)} type="button">Sign In</button>
```

### Callback Events

To handle the result of auth there are three callback events. 

- `onSuccess` - On Successful authentication, this includes the user authenticated along with the passed state and context to the Kinde hook
- `onError` - When an error occurs during authentication, this includes the error along with the passed state and context to the Kinde hook

```jsx
<KindeProvider
    callbacks={
      {
        onSuccess: (user, state, context) => console.log("onSuccess", user, state, context),
        onError: (error, state, context) => console.log("onError", error, state, context),
      }
    }
>
  <Routes />
</KindeProvider>
```

### Passing additional params to the auth url

Both the `login` and `register` methods accept all the extra authentication params that can be passed to Kinde as part of the auth flow.  All the params are also accepted as attributes to the `LoginLink` and `RegisterLink` components.

Some things you may wish to pass are:

- `loginHint` this allows you to ask Kinde to prepopulate a users email address on the sign-up and sign-in screens.
- `lang` if you offer multi-language support Kinde will automatically figure out the best language for your user based on their browser. However, if you want to force a language and override the users preference, you can do so by passing this attribute.

```javascript
    login({
      loginHint: "jenny@example.com",
      lang: "ru"
    })
```

### Signing out

Kinde provides a `useKindeAuth` hook with the method `logout` and component `LogoutLink` which can be used to end the current session.

```jsx
const { logout } = useKindeAuth();

<button onClick={() => logout(/* params here */)} type="button">Sign out</button>
```

```jsx
import { LogoutLink } from '@kinde-oss/kinde-auth-react/components';

<LogoutLink>Sign out</LogoutLink>
```

## Test sign up

Register your first user by signing up yourself. You’ll see your newly registered user on the **Users** page in Kinde.

### **View user profile**

You can get an authorized user’s profile from any component using the Kinde React hook.

```jsx
import { useKindeAuth } from '@kinde-oss/kinde-auth-react';
const SayHello = () => {
  const { user } = useKindeAuth();
  return <p>Hi {user.firstName}!</p>;
};
```

To be on the safe side we have also provided `isAuthenticated` and `isLoading` state to prevent rendering errors.

```jsx
import { useKindeAuth } from '@kinde-oss/kinde-auth-react';

const UserProfile = () => {
  const { user, isAuthenticated, isLoading } = useKindeAuth();

  if (isLoading) {
    return <p>Loading</p>;
  }

  return ({
    isAuthenticated ?
      <div>
	<h2>{user.firstName}</h2>
	<p>{user.preferredEmail}</p>
      </div> 
    :
      <p>Please sign in or register!</p>
  });
};
```

## Access control

### Check if user is authenticated

```jsx
const { isAuthenticated } = useKindeAuth();
```

### Check user rights

There are two ways to check if a user has a specific permission:

- Using the `has` function from the `@kinde-oss/kinde-auth-react/utils` package
- Using the `<ProtectedRoute>` component

#### Using the `has` function

The `has` function is a helper function that checks if a user has specific roles, permissions, feature flags, or billing entitlements. It returns a boolean value indicating whether the user meets all the specified criteria.

```typescript
import { has } from '@kinde-oss/kinde-auth-react/utils';

// Basic usage - check multiple criteria at once
const userHasAccess = has({
  roles: ['admin'],
  permissions: ['create:todos'],
  featureFlags: ['theme'],
  billingEntitlements: ['premium']
});
```

**Parameters:**
- `roles` (optional): Array of role names the user must have
- `permissions` (optional): Array of permission names the user must have  
- `featureFlags` (optional): Array of feature flag names the user must have
- `billingEntitlements` (optional): Array of billing entitlement names the user must have

**Return value:** `boolean` - `true` if user meets all criteria, `false` otherwise

### API vs Token-based checks

By default, the `has` function performs checks using the user's tokens. You can override this behavior by passing the `forceApi` option to perform server-side validation:

```typescript
// Force all checks to use API calls instead of tokens
has({
  roles: ['admin'],
  permissions: ['create:todos'],
  forceApi: true
})

// Force only specific checks to use API calls
has({
  roles: ['admin'],
  permissions: ['create:todos'],
  forceApi: { permissions: true } // Only permissions use API, roles use tokens
})
```

### Advanced usage with conditions

You can add custom conditions to any check by expanding the object structure:

```typescript
// Add custom conditions to role checks
has({
  roles: [{ 
    role: 'admin', 
    condition: (user) => user.isAdmin && user.isActive 
  }],
})

// Add custom conditions to permission checks
has({
  permissions: [{ 
    permission: 'create:todos', 
    condition: (user) => user.organizationId === 'org123' 
  }],
})
```

### Feature flag value checking

For feature flags, you can check both the flag's existence and its specific value:

```typescript
// Check if feature flag exists
has({
  featureFlags: ['theme']
})

// Check if feature flag has a specific value
has({
  featureFlags: [{ flag: 'theme', value: 'dark' }]
})

// Check multiple feature flags with values
has({
  featureFlags: [
    { flag: 'theme', value: 'dark' },
    { flag: 'beta_features', value: true }
  ]
})
```

### Complete example

```typescript
import { has } from '@kinde-oss/kinde-auth-react/utils';

const checkUserAccess = () => {
  const canAccessAdminPanel = has({
    roles: ['admin', 'super_admin'],
    permissions: ['read:users', 'write:users'],
    featureFlags: ['admin_panel'],
    billingEntitlements: ['enterprise_plan']
  });

  const canUseDarkTheme = has({
    featureFlags: [{ flag: 'theme', value: 'dark' }],
    permissions: ['customize:theme']
  });

  return { canAccessAdminPanel, canUseDarkTheme };
};
```

### TypeScript type safety

You can enhance type safety by declaring your specific roles, permissions, feature flags and billing entitlements. This provides autocomplete and compile-time checking for your access control.

Create a type declaration file (e.g., `kinde-types.d.ts`) in your project:

```typescript
declare module "@kinde-oss/kinde-auth-react/utils" {
  interface KindeConfig {
    roles: ['admin', 'user', 'moderator', 'super_admin'];
    permissions: ['read:users', 'write:users', 'delete:users', 'manage:settings'];
    featureFlags: ['dark_mode', 'beta_features', 'admin_panel'];
    billingEntitlements: ['basic', 'premium', 'enterprise'];
  }
}
```

**Note:** Make sure your `tsconfig.json` includes the type declaration file, or place it in a directory that TypeScript automatically includes (like `src/types/` or the root of your project).

### Using the `<ProtectedRoute>` component

**Note:** The `<ProtectedRoute>` component requires the `react-router-dom` package to be installed.

The `<ProtectedRoute>` component is a wrapper component that checks if a user has specific permissions and renders the child component if they do. If the user doesn't have the required permissions, they are redirected to a fallback path.

```jsx
import { ProtectedRoute } from '@kinde-oss/kinde-auth-react/react-router';

// Basic usage - protect a route with role-based access
<ProtectedRoute has={{ roles: ['admin'] }} fallbackPath="/">
  <div>You have access to this admin page</div>
</ProtectedRoute>
```

**Props:**
- `has` (required): Object containing the access requirements (same format as the `has` function)
- `fallbackPath` (required): Path to redirect to if user doesn't have access
- `children` (required): React components to render if user has access

#### Basic examples

```jsx
// Protect with single role
<ProtectedRoute has={{ roles: ['admin'] }} fallbackPath="/unauthorized">
  <AdminDashboard />
</ProtectedRoute>

// Protect with multiple roles (user must have at least one)
<ProtectedRoute has={{ roles: ['admin', 'moderator'] }} fallbackPath="/">
  <ModeratorPanel />
</ProtectedRoute>

// Protect with permissions
<ProtectedRoute has={{ permissions: ['read:users', 'write:users'] }} fallbackPath="/dashboard">
  <UserManagement />
</ProtectedRoute>

// Protect with feature flags
<ProtectedRoute has={{ featureFlags: ['beta_features'] }} fallbackPath="/">
  <BetaFeatures />
</ProtectedRoute>
```

#### Complex access control

```jsx
// Multiple criteria - user must have ALL specified requirements
<ProtectedRoute 
  has={{ 
    roles: ['admin'],
    permissions: ['manage:users'],
    featureFlags: ['admin_panel'],
    billingEntitlements: ['premium']
  }} 
  fallbackPath="/upgrade"
>
  <PremiumAdminPanel />
</ProtectedRoute>

// Using API-based checks for real-time validation
<ProtectedRoute 
  has={{ 
    roles: ['admin'],
    permissions: ['manage:users'],
    forceApi: true
  }} 
  fallbackPath="/"
>
  <RealTimeAdminPanel />
</ProtectedRoute>
```

#### Integration with React Router

```jsx
import { Routes, Route } from 'react-router-dom';
import { ProtectedRoute } from '@kinde-oss/kinde-auth-react/react-router';

function App() {
  return (
    <Routes>
      <Route path="/" element={<HomePage />} />
      <Route 
        path="/admin" 
        element={
          <ProtectedRoute has={{ roles: ['admin'] }} fallbackPath="/">
            <AdminPage />
          </ProtectedRoute>
        } 
      />
      <Route 
        path="/premium" 
        element={
          <ProtectedRoute 
            has={{ billingEntitlements: ['premium'] }} 
            fallbackPath="/upgrade"
          >
            <PremiumPage />
          </ProtectedRoute>
        } 
      />
    </Routes>
  );
}
```

#### Complete example with multiple protected routes

```jsx
import { Routes, Route } from 'react-router-dom';
import { ProtectedRoute } from '@kinde-oss/kinde-auth-react/react-router';

function App() {
  return (
    <Routes>
      <Route path="/" element={<HomePage />} />
      
      {/* Basic admin access */}
      <Route 
        path="/admin" 
        element={
          <ProtectedRoute has={{ roles: ['admin'] }} fallbackPath="/">
            <AdminDashboard />
          </ProtectedRoute>
        } 
      />
      
      {/* Premium features */}
      <Route 
        path="/premium" 
        element={
          <ProtectedRoute 
            has={{ billingEntitlements: ['premium'] }} 
            fallbackPath="/upgrade"
          >
            <PremiumFeatures />
          </ProtectedRoute>
        } 
      />
      
      {/* Beta features with specific permissions */}
      <Route 
        path="/beta" 
        element={
          <ProtectedRoute 
            has={{ 
              featureFlags: ['beta_access'],
              permissions: ['beta:test']
            }} 
            fallbackPath="/"
          >
            <BetaFeatures />
          </ProtectedRoute>
        } 
      />
    </Routes>
  );
}
```

## Call your API

The `getAccessToken` method lets you to securely call your API and pass the bearer token to validate that your user is authenticated.

```jsx
const { getAccessToken } = useKindeAuth();

const fetchData = async () => {
  try {
    const accessToken = await getAccessToken();
    const res = await fetch(`<your-api>`, {
      headers: {
        Authorization: `Bearer ${accessToken}`
      }
    });
    const {data} = await res.json();
    console.log({data});
  } catch (err) {
    console.log(err);
  }
};
```

We have a range of backend SDKs available to assist you in securing your back end application using this token, alternatively you can use any JWT validator and decoder to assit you here.

## Audience

An `audience` is the intended recipient of an access token - for example the API for your application. The `audience` argument can be passed to the Kinde client to request an audience be added to the provided token.

The audience of a token is the intended recipient of the token.

```jsx
<KindeProvider
  audience="<your_api>
>
```

To request multiple audiences, pass them separated by white space.

```jsx
<KindeProvider
  audience="<your_api1> <your_api2>
>
```

For details on how to connect, see [Register an API](/developer-tools/your-apis/register-manage-apis/).

## Organizations

### Create an organization

To create a new organization with the user registration, you can use the `createOrg` function to start the registration process:

```jsx
<LoginLink isCreateOrg={true}>Sign in</LoginLink>
<button onClick={() => login({isCreateOrg: true})} type="button">Sign in</button>
```

### Sign up/sign in users to organizations

To sign up a user to a particular organization, you must pass the `orgCode` from your Kinde account as the user is created. You can find the `orgCode` on the **Details** page of each organization in Kinde.

```jsx
<LoginLink orgCode="org_1234">Sign in</LoginLink>
<button onClick={() => login({orgCode: "org_1234"})} type="button">Sign in</button>
```

Following authentication Kinde provides a json web token (jwt) to your application. Along with the standard information we also include the `org_code` and the `permissions` for that organization (this is important as a user can belong to multiple organizations and have different permissions for each). Example of a returned token:

```json
{
  "aud": ["https://your_subdomain.kinde.com"],
  "exp": 1658475930,
  "iat": 1658472329,
  "iss": "https://your_subdomain.kinde.com",
  "jti": "123457890",
  "org_code": "org_1234",
  "permissions": ["read:todos", "create:todos"],
  "scp": ["openid", "offline"],
  "sub": "kp:123457890"
}
```

For more information about how organizations work in Kinde, see [Kinde organizations for developers](/build/organizations/orgs-for-developers/).

## User Permissions

Once a user has been verified as signed in, your project/application will be returned in the JWT token with an array of permissions for that user. You need to configure your project to read permissions and unlock the respective functions.

[Configure permissions](/manage-users/roles-and-permissions/user-permissions/) in Kinde first. Here is an example set of permissions.

```json
"permissions":[
    "create:todos",
    "update:todos",
    "read:todos",
    "delete:todos",
    "create:tasks",
    "update:tasks",
    "read:tasks",
    "delete:tasks",
]
```

We provide helper functions to more easily access permissions:

```jsx
const {getPermission, getPermissions} = useKindeAuth();

await getPermission("create:todos");
// {permissionKey: "create:todos", orgCode: "org_1234", isGranted: true}

await getPermissions();
// {orgCode: "org_1234", permissions: ["create:todos", "update:todos", "read:todos"]}
```

A practical example in code might look something like:

```jsx
{
  (await getPermission("create:todos")).isGranted ? <button>Create todo</button> : null;
}
```

## **Feature flags**

When a user signs in, the access token your project/application receives contains a custom claim called `feature_flags` which is an object detailing the feature flags for that user.

You can [set feature flags](/releases/feature-flags/add-feature-flag/) in your Kinde account. Here’s an example.

```jsx
feature_flags: {
  theme: {
      "t": "s",
      "v": "pink"
 },
 is_dark_mode: {
      "t": "b",
      "v": true
  },
 competitions_limit: {
      "t": "i",
      "v": 5
  }
}
```

In order to minimize the payload in the token we have used single letter keys / values where possible. The single letters represent the following:

`t` = `type`

`v` = `value`

`s` = `string`

`b` = `boolean`

`i` = `integer`

We provide helper functions to more easily access feature flags:

```jsx
const { getFlag } = useKindeAuth();

/* Example usage */
await getFlag('theme'); // string value
await getFlag<boolean>('theme'); // boolean value
await getFlag<number>('theme'); // numeric value

/*{
//   "code": "theme",
//   "type": "string",
//   "value": "pink",
//   "is_default": false // whether the fallback value had to be used
*/}

getFlag('create_competition', {defaultValue: false});
/*{
      "code": "create_competition",
      "value": false,
      "is_default": true // because fallback value had to be used
}*/
```

A practical example in code might look something like:

```jsx
const { getFlag } = useKindeAuth();

{
  (await getFlag<boolean>('create_competition')) ? <button>Create competition</button> : null;
}
```

A practical example in code might look something like:

```jsx
const {getFlag} = useKindeAuth();

{
  getFlag<boolean>("create_competition") ? (
    <button className={`theme-${getFlag("theme")}`}>Create competition</button>
  ) : null;
}
```

## **Overriding scope**

By default the JavaScript SDK requests the following scopes:

- `profile`
- `email`
- `offline`
- `openid`

You can override this by passing `scope` into the `<KindeProvider>`.

```jsx
<KindeProvider
  scope="openid
>
```

## **Getting claims**

We have provided a helper to grab any claim from your ID or access tokens. The helper defaults to access tokens:

```jsx
const { getClaim } = useKindeAuth();

getClaim("aud");
// {name: "aud", "value": ["api.yourapp.com"]}

getClaim("given_name", "idToken");
// {name: "given_name", "value": "David"}
```

## **Persisting authentication state on page refresh or new tab**

You will find that when you refresh the browser using a front-end based SDK that the authentication state is lost. This is because there is no secure way to persist this in the front-end.

There are two ways to work around this.

- (Recommended) use our [Custom Domains](/build/domains/pointing-your-domain/) feature which then allows us to set a secure, httpOnly first party cookie on your domain.
- (Non-production solution only) If you’re not yet ready to add your custom domain, or for local development, we offer an escape hatch `<KindeProvider>` `useInsecureForRefreshToken`. This will use local storage to store the refresh token. DO NOT use this in production.

Once you implement one of the above, you don’t need to do anything else.

## **Persisting application state**

The options argument passed into the `login` and `register` methods accepts an `state` key where you can pass in the current application state prior to redirecting to Kinde. This is then returned to you in the second argument of the `onSuccess` callback as seen above.

A common use case is to allow redirects to the page the user was trying to access prior to authentication. This could be achieved as follows:

Login handler:

```jsx
<button
  onClick={() =>
    login({
      state: {
        redirectTo: location.state ? location.state?.from?.pathname : null
      }
    })
  }
/>
```

Redirect handler:

```jsx
<KindeProvider
  callbacks={
    {
      onSuccess: (user, state, context) => {
        window.location = state?.redirectTo
      },
    }
  }
>
```

## **Token storage in the authentication state**

By default the JWTs provided by Kinde are stored in memory. This protects you from both [CSRF](https://owasp.org/www-community/attacks/csrf) attacks (possible if stored as a client side cookie) and [XSS](https://owasp.org/www-community/attacks/xss/) attacks (possible if persisted in local storage).

The trade off with this approach however is that if a page is refreshed or a new tab is opened then the token is wiped from memory, and the sign in button would need to be clicked to re-authenticate. There are two ways to prevent this behaviour:

1. Use the Kinde custom domain feature. We can then set a secure, httpOnly cookie against your domain containing only the refresh token which is not vulnerable to CSRF attacks.
2. There is an escape hatch which can be used for local development: `useInsecureForRefreshToken`. This SHOULD NOT be used in production. We recommend you use a custom domain. This will store only the refresh token in local storage and is used to silently re-authenticate.

```jsx
<KindeProvider
  useInsecureForRefreshToken={process.env.NODE_ENV === 'development'}
  ...
>
```

## Migration from v4 to v5

### login and register method params

No longer need to use authUrlParams parameter, all url params are now passed at top level and are now camel case instead of snake case.

e.g.

```    
login({
  authUrlParams: {
    login_hint: "jenny@example.com",
    lang: "ru"
  }
})
```
becomes
```    
login({
  loginHint: "jenny@example.com",
  lang: "ru"
})
```

### Callbacks

`onRedirectCallback` has been removed in favor of a richer event system. 

This has been replaced by `callbacks > onSuccess`

### Create organization

`createOrg` method has been removed and replaced by using `isCreateOrg` on the login or register methods or Components

### Accessing access token

`getToken` has been replaced by `await getAccessToken`

### Token helpers

all token helpers are now async methods

### getClaims 

token attribute changed to camelCase.

e.g.
```javascript
getClaim("given_name", "id_token");
```
becomes
```javascript
await getClaim("given_name", "idtoken");
```


## **API References - KindeProvider**

### **`audience`**

The audience claim for the JWT.

Type: `string`

Required: No

### **`clientId`**

The ID of your application as it appears in Kinde.

Type: `string`

Required: Yes

### **`domain`**

Either your Kinde instance url or your custom domain. e.g `https://yourapp.kinde.com`

Type: `string`

Required: Yes

### **`logoutUri`**

Where your user will be redirected when they log out.

Type: `string`

Required: No

### `useInsecureForRefreshToken`

An escape hatch for storing the refresh in local storage for local development.

Type: `boolean`

Required: No

Default: `false`

### **`redirectUri`**

The URL that the user will be returned to after authentication.

Type: `string`

Required: Yes

### **`scope`**

The scopes to be requested from Kinde.

Type: `string`

Required: No

Default: `openid profile email offline`

## **API References- useKindeAuth hook**

### `getClaim`

Gets a claim from an access or ID token.

Arguments:

```typescript
claim: string, tokenKey?: "accessToken" | "idToken"
```

Usage:

```jsx
getClaim("givenName", "idToken");
```

Sample:

```jsx
"David";
```

### `getOrganization`

Get details for the organization your user is signed into.

Usage:

```jsx
getOrganization();
```

Sample:

```jsx
{
  orgCode: "org_1234";
}
```

### `getPermission`

Returns the state of a given permission.

Arguments:

```jsx
key: string;
```

Usage:

```jsx
getPermission("read:todos");
```

Sample:

```jsx
{
	orgCode: "org_1234",
	isGranted: true
}
```

### `getPermissions`

Returns all permissions for the current user for the organization they are signed into.

Usage:

```jsx
getPermissions();
```

Sample:

```jsx
{
	orgCode: "org_1234",
	permissions: [
		"create:todos",
		"update:todos",
		"read:todos
	]
}
```

### `getToken`

Returns the raw Access token from memory.

Usage:

```jsx
getToken();
```

Sample:

```jsx
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c;
```

### `getUserProfile`

Returns the profile for the current user.

Usage:

```jsx
getUserProfile();
```

Sample:

```jsx
{
  givenName: "Dave";
  id: "abcdef";
  familyName: "Smith";
  email: "dave@smith.com";
}
```

### `getUserOrganizations`

Gets an array of all organizations the user has access to.

Usage:

```jsx
getUserOrganizations();
```

Sample:

```jsx
{
  orgCodes: ["org_1234", "org_5678"];
}
```

### `login`

Constructs redirect url and sends user to Kinde to sign in.

Arguments:

```typescript
orgCode?: string;
state?: object;
```

Usage:

```jsx
login();
```

Sample:

```jsx
redirect;
```

### `logout`

Logs the user out of Kinde.

Argument:

```typescript
orgCode?: string
```

Usage:

```jsx
logout();
```

Sample:

```jsx
redirect;
```

### `register`

Constructs redirect url and sends user to Kinde to sign up.

Arguments:

```typescript
orgCode?: string;
state?: object;
```

Usage:

```jsx
register();
```

Sample:

```jsx
redirect;
```

## **API References- login**

### `getClaim`

Gets a claim from an access or ID token.

Arguments:

```typescript
claim: string, tokenKey?: "accessToken" | "idToken"
```

Usage:

```jsx
getClaim("given_name", "idToken");
```

Sample:

```jsx
"David";
```

If you need any assistance with getting Kinde connected reach out to us at [support@kinde.com](mailto:support@kinde.com).
