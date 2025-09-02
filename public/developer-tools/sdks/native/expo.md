
The Kinde React Native SDK allows developers to quickly and securely integrate a new or an existing React Native application into the Kinde platform. This SDK is for people using Expo.

## Register for Kinde

If you haven't already got a Kinde account, [register for free here](https://app.kinde.com/register) (no credit card required). Registering gives you a Kinde domain, which you need to get started, e.g. `yourapp.kinde.com`.

## Before you install

You will need Node, the React Native command line interface, a JDK, Android Studio (for Android) and Xcode (for iOS).

Follow [the installation instructions for your chosen OS](https://reactnative.dev/docs/environment-setup) to install dependencies.

## Installation with Expo Managed Workflow

### Install package

<PackageManagers pkg="@kinde/expo" />

### Setup provider

```jsx
<KindeAuthProvider
  config={{
    domain: "<KINDE DOMAIN>",  // e.g https://mybusiness.kinde.com
    clientId: "<CLIENT ID>",
  }}
  // All callbacks are optional
  callbacks={{
    onSuccess: async (token, state, context) => {
    },
    onError: (error) => {
    },
    onEvent: async (event, state, context) => {
    },
  }}
>
  {/* Your app components go here */}  
</KindeAuthProvider>
```

### Auth Methods

```jsx
const kinde = useKindeAuth();

const handleSignUp = async () => {
  const token = await kinde.register();

  if (token) {
    // User was authenticated
  }
};

const handleSignIn = async () => {
  const token = await kinde.login();
  if (token) {
    // User was authenticated
  }
};

const handleLogout = async () => {
  console.log("logout", await kinde.logout());
};
```

#### Example usage

```jsx
<Pressable onPress={handleSignIn}>
	<ThemedText>Sign In</ThemedText>
</Pressable>
```

Login and register methods accept and object containing all [Kinde supported URL parameters](https://docs.kinde.com/developer-tools/about/using-kinde-without-an-sdk/#request-parameters).

Logout accepts object which allows you to revoke the token

```jsx
kinde.logout({ revokeToken: true })
```

#### Properties (Only available from `useKindeAuth`)

- `isAuthenticated` - Returns true/false if the user is authenticated

## Kinde configuration

1. In Kinde, go to **Settings > Applications.**
2. Select **View details** on the **Frontend app**.
3. Scroll down to the **Callback URLs** section.
4. Add in the callback URLs for your React Native app, which should look something like this:
   - Allowed callback URLs - `<myapp://localhost:3000>`
   - Allowed logout redirect URLs - `<myapp://localhost:3000>`


Make sure you press the Save button at the bottom of the page!

Note: The `<myapp://localhost:3000>` is used as an example of local URL Scheme, change to the local URL Scheme or production URL Scheme that you use.

## Token Utilities

A selection of utility functions are available. 

*Expo 53+*: Import from `@kinde/expo/utils` and `useKindeAuth` hook
*Expo 51 and 52*: Import from `@kinde/js-utils` and `useKindeAuth` hook

```ts
import { getUserProfile, getFlag, getRoles } from "@kinde/expo/utils";

// Example usage
const checkUserProfile = async () => {
  const profile = await getUserProfile();
  console.log("User profile:", profile);
};
```

#### Utility functions include:

- `getDecodedToken` - Get token decoded values
- `getUserProfile` - Get the current user's profile
- `getFlag` - Check feature flag values
- `getRoles` - Get the current user's roles
- `getCurrentOrganization` - Get the current organization
- `getUserOrganizations` - Get all organizations the user belongs to
- `getPermission` - get a single permission value
- `getPermissions` - get all user permissions
- `getClaim` - Get a specific claim from the token
- `getClaims` - Get all claims from the token
- `refreshToken` - Manually refresh the access token


### `getDecodedToken`

Get the decoded access token or ID token.

```typescript
getDecodedToken = async <T = JWTDecoded>(
  tokenType: "accessToken" | "idToken" = StorageKeys.accessToken,
): Promise<(T & JWTDecoded) | null>
```

Example usage:
```javascript
// Get the decoded access token
const decodedAccessToken = await getDecodedToken("accessToken");
// Get the decoded ID token
const decodedIdToken = await getDecodedToken("idToken");

// Adding custom claims to the decoded token
const decodedAccessTokenWithCustomClaims = await getDecodedToken<{
  customClaim: string;
}>("accessToken");
```

### `getClaim`

Gets a claim from an access or ID token.

```typescript
getClaim = async <T = JWTDecoded, V = string | number | string[]>(
  keyName: keyof T,
  tokenType: "accessToken" | "idToken" = "accessToken",
): Promise<{
  name: keyof T;
  value: V;
} | null>
```

Example usage:
```javascript
// Get the decoded access token
const roles = await getClaim("roles", "accessToken");
// Get the decoded ID token
const givenName = await getClaim("given_name", "idToken");

// Acessing custom claims
const decodedAccessTokenWithCustomClaims = await getClaim<{
  customClaim: string;
}>("customClaim", "accessToken");
```

### `getCurrentOrganization`

Returns the current users logged in organization code.

```typescript
getCurrentOrganization = async (): Promise<string | null>
```

Example usage:
```javascript
// Get the decoded access token
const orgCode = await getCurrentOrganization();
// org_123456
```

### `getUserOrganizations`

Returns all organization codes the current user belongs to.

```typescript
getUserOrganizations = async (): Promise<string[] | null>
```

Example usage:
```javascript
// Get the decoded access token
const orgCode = await getUserOrganizations();
// [
//   "org_0000000000001", 
//   "org_0000000000002", 
//   "org_0000000000003", 
//   "org_0000000000004
// ]
```

### `getFlag`

Get the value of a feature flag.

```typescript
getFlag = async <T = string | boolean | number | object>(
  name: string,
): Promise<T | null>
```

Example usage:
```javascript
// Get the feature flag value
const featureValue = await getFlag("feature_flag_name");

// Define the type of the feature flag
const featureValue = await getFlag<string>("feature_flag_name");
const featureValue = await getFlag<boolean>("feature_flag_name");
const featureValue = await getFlag<number>("feature_flag_name");
const featureValue = await getFlag<object>("feature_flag_name");
```

### `getPermission`

Get the value of a feature flag.

```typescript
getPermission = async <T = string>(
  permissionKey: T,
): Promise<PermissionAccess>
```

Example usage:
```javascript
// Get the feature flag value
const permission = await getPermission("feature_flag_name");
// {
//   permissionKey: "feature_flag_name",
//   orgCode: "org_123456",
//   isGranted: true / false,
// }
```


### `getPermissions`

Get the permissions for the current user for the organization they are signed into.

```typescript
getPermissions = async <T = string>(
  permissionKey: T,
): Promise<PermissionAccess>
```

Example usage:
```javascript
// Get the feature flag value
const permissions = await getPermissions("feature_flag_name");
// {
//   orgCode: "org_123456",
//   permissions: [
//     "create:todos",
//     "update:todos",
//     "read:todos",
//     "delete:todos",
//     "create:tasks",
//     "update:tasks",
//     "read:tasks",
//     "delete:tasks
//   ]
// }
```

### `getRoles`

Get the users Roles

<Aside>
**Note:** Roles are optional in the token, will need to add to the token in your application settings
</Aside>

```typescript
getRoles = async (): Promise<Role[]>
```

Example usage:
```javascript
// Get the feature flag value
const roles = await getRoles();
// [
//   {
//     id: "01932730-c828-c01c-9f5d-c8f15be13e24",
//     key: "admin",
//     name: "admin",
//   },
// ]
```

### `getUserProfile`

```typescript
getUserProfile = async <T>(): Promise<
  (UserProfile & T) | null
>
```

Example usage:
```javascript
// Get the feature flag value
const roles = await getUserProfile();
// {
//   "email": "someuser@emaildomain.com", 
//   "familyName": "Bloggs", 
//   "givenName": "Joe", 
//   "id": "kp_1234...", 
//   "picture": "https://example.com/image.png",
// }
```

### `refreshToken`

```typescript
refreshToken = async ({
  domain,
  clientId,
  refreshType = RefreshType.refreshToken,
  onRefresh,
}: {
  domain: string;
  clientId: string;
  refreshType?: RefreshType;
  onRefresh?: (data: RefreshTokenResult) => void;
}): Promise<RefreshTokenResult>
```

Example usage:
```javascript
const refreshToken = await refreshToken({
  domain: "https://mybusiness.kinde.com",
  clientId: "client_id"
});
// {
//   accessToken: "eyJhbGciOiJSUzI1N...",
//   idToken: "eyJhbGciOiJSUzI1N...",
//   refreshToken: "E5hxe-AOSTcFjri3YV...",
//   success: true,
// }
```

### **Caching Issues**

Sometimes there will be issues related to caching when you develop React Native.
There are some recommendations for cleaning the cache:

1. Remove `node_modules`, `yarn.lock` or `package-lock.json`.
2. Clean cache: `yarn cache clean` or `npm cache clean --force`.
3. Make sure you have changed values in `.env` file.
4. Trying to install packages again: `yarn install` or `npm install`.
5. Run Metro Bundler: `yarn start --reset-cache` or `npm start --reset-cache`.

Assume your StarterKit path is `<StarterKit_PATH>`.

**Clean cache for Android**

1. Run this:

   ```shell
   cd <StarterKit_PATH>/android
   ./gradlew clean

   ```

**Clean cache for iOS**

1. Run this:

   ```shell
   cd <StarterKit_PATH>/ios
   rm -rf Pods && rm Podfile.lock

   ```

2. Clean build folders on **Xcode**.

If you need help connecting to Kinde, please contact us at [support@kinde.com](mailto:support@kinde.com).
