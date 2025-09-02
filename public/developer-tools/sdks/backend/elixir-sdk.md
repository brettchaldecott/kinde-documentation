
The Kinde Elixir SDK allows developers to connect their Elixir app to Kinde. This document is relevant for up to Elixir v1.2.0.

You can also view the [Elixir docs](https://github.com/kinde-oss/kinde-elixir-sdk) and [Elixir starter kit](https://github.com/kinde-starter-kits/elixir-starter-kit) in GitHub.

## Register for Kinde

If you haven’t already got a Kinde account, [register for free here](https://app.kinde.com/register) (no credit card required). Registering gives you a Kinde domain, which you need to get started, e.g. `yourapp.kinde.com`.

## Install

Install erlang and elixir. Update the deps, update path to the SDK in `mix.exs`.

```elixir
{:kinde_sdk, "~> 1.2.0"}
```

Add to your extra applications in `mix.exs`.

```elixir
def application do
   [
     extra_applications: [:logger, :kinde_sdk]
   ]
end
```

## Set callback URLs

1. In Kinde, go to **Settings > Applications > [Your app] > View details**.
2. Add your callback URLs in the relevant fields. For example:
   - Allowed callback URLs (also known as redirect URIs) - for example, `http://localhost:4000/callback`
   - Allowed logout redirect URLs - for example, `http://localhost:4000`
3. Select **Save**.

**Note:** The `http://localhost:4000` is an example of a commonly used local development URL. It should be replaced with the URL where your app is running.

## Add environments

Kinde comes with a production environment, but you can set up other environments if you want to. Note that each environment needs to be set up independently, so you need to use the Environment subdomain in the code block above for those new environments.

## Configure your app

**API Keys**

Follow these steps to set up the project:

1. Create a .env file at the root directory.
2. Add the following, but change the `“values”` to match your own information.

```elixir
export KINDE_BACKEND_CLIENT_ID="test_x1y2z3a1
export KINDE_FRONTEND_CLIENT_ID="test_a1b2c3d4
export KINDE_CLIENT_SECRET="test_112233
export KINDE_REDIRECT_URL="http://text.com/callback
export KINDE_DOMAIN="https://test.kinde.com
export KINDE_LOGOUT_REDIRECT_URL="http://text.com/logout
export KINDE_PKCE_LOGOUT_URL="http://test.com/logout
export KINDE_PKCE_REDIRECT_URL="http://test.com/pkce-callback
export KINDE_BASE_URL="https://app.kinde.com
```

1. If required, set the scopes. You can include scopes such as `openid` `profile` `offline`, in addition to `email`.

```elixir
config :kinde_sdk, scope: "email"
```

1. Open the console and write `source .env` before any mix command.

**Environment variables**

You can also set these variables in .env file within your project directory. The following variables need to be replaced in the code snippets.

- `KINDE_HOST` - your Kinde domain - e.g. `https://yourkindedomain.kinde.com`
- `KINDE_REDIRECT_URL` - your callback url, make sure this URL is under your allowed callback redirect URLs. - e.g. `http://localhost:4000/callback`
- `KINDE_POST_LOGOUT_REDIRECT_URL` - where you want users to be redirected to after logging out, make sure this URL is under your allowed logout redirect URLs. - e.g. `http://localhost:4000`
- `KINDE_CLIENT_ID` - you can find this on the **Application details** page
- `KINDE_CLIENT_SECRET` - you can find this on the **Application details** page

## Usage

Execute following in your terminal to run:

```elixir
mix deps.get
mix phx.server
```

## **OAuth Flows (Grant Types)**

The KindeClientSDK struct implements three OAuth flows:

- Client Credentials flow
- Authorization Code flow
- Authorization Code with PKCE flow

Each flow can be used with their corresponding grant type when initializing a client.

| **OAuth Flow**               | **Grant Type**                | **Type** |
| ---------------------------- | ----------------------------- | -------- |
| Client Credentials           | :client_credentials           | atom     |
| Authorization Code           | :authorization_code           | atom     |
| Authorization Code with PKCE | :authorization_code_flow_pkce | atom     |

## Integrate with your app

Create a new instance of the Kinde Auth client object before you initialize your app:

```elixir
{conn, client} =
  KindeClientSDK.init(
    conn,
    Application.get_env(:kinde_sdk, :domain),
    Application.get_env(:kinde_sdk, :redirect_url),
    Application.get_env(:kinde_sdk, :backend_client_id),
    Application.get_env(:kinde_sdk, :client_secret),
    :client_credentials,
    Application.get_env(:kinde_sdk, :logout_redirect_url)
  )
```

## Log in and registration

The Kinde client provides methods for easy log in and registration.

You can add buttons in your HTML as follows:

```html
<div class="navigation">
  <a href="/login" type="button">Login</a>
  <a href="/register" type="button">Register</a>
</div>
```

You will also need to route `/login` and `/register` to the SDK methods:

```elixir
conn = KindeClientSDK.login(conn, client)
conn = KindeClientSDK.register(conn, client)
```

## **Test sign up**

Register your first user by signing up yourself. You’ll see your newly registered user on the **Users** page of the relevant organization (or default organization) in Kinde.

## Manage redirects

When the user is redirected back to your site from Kinde, this will call your callback URL defined in the `KINDE_REDIRECT_URL` variable. You will need to route `/callback` to call a function to handle this.

```elixir
def callback(conn, _params) do
    {conn, client} = KindeClientSDK.get_token(conn)

    data = KindeClientSDK.get_all_data(conn)
end
```

## **Tokens**

We use the Kinde helper function to get the tokens generated by `login` and `get_token`.

```elixir
data = KindeClientSDK.get_all_data(conn)
IO.inspect(data.login_time_stamp, label: "Login Time Stamp")
```

Or first calling the `get_token` function:

```elixir
{conn, client} = KindeClientSDK.get_token(conn)
```

Example of a returned token:

```elixir
%{
	"access_token" => "eyJhbGciOiJSUzI1...",
	"expires_in" => 89274,
	"scope" => "openid profile email offline",
	"token_type" => "bearer
}
```

## **User details**

This function returns the user object including Kinde ID. This function reads the information from the `id_token` that is returned after successful authentication. Include the required scopes if not added already (`openid profile email offline`).

```elixir
KindeClientSDK.get_user_detail(conn)
```

**Note:** You need to have already authenticated before you call the API, otherwise an error will occur.

## **View users in Kinde**

Go to the **Users** page in Kinde to see who has registered.

## O**rganizations**

**Create an organization**

There is an additional `create_org` method which allows an organization to be created. This method calls the current sign-up logic by setting the `is_create_org` parameter to true.

Use this helper function to create an organization.

```elixir
conn = KindeClientSDK.create_org(conn, client)
conn = KindeClientSDK.create_org(conn, client)
```

**Sign up and sign in to organizations**

Kinde has a unique code for every organization. You’ll have to pass this code when creating a client or registering a new user: `additional_parameters_org_code`.

If you want a user to sign in to a particular organization, pass this code along with the sign in method.

For more information about how organizations work in Kinde, see [Kinde organizations for developers](/build/organizations/orgs-for-developers/).

## **Logout**

The Kinde SDK client comes with a logout method.

```elixir
conn = KindeClientSDK.logout(conn)
```

## **Authenticated**

Returns whether if a user is signed in by verifying that the access token is still valid.

```elixir
KindeClientSDK.authenticated?(conn)
```

## **Claims**

We have provided a helper to grab any claim from your ID or access tokens, which accepts a key for a token and returns the claim value. There is also an optional argument to define which token to check. The helper defaults to access tokens.

```elixir
KindeClientSDK.get_claims(conn)

KindeClientSDK.get_claim(conn, "jti", :id_token)
```

This function will returns claims as follows.

```elixir
%{
	"aud" => [],
	"azp" => "",
	"exp" => 1649314,
	"gty" => ["client_credentials"],
	"iat" => 1662914,
	"iss" => "https://smith.com",
	"jti" => "7fa15b8-086-495-bba-b191b2aa2f",
	"scp" => ["openid", "profile", "email", "offline"]
}
```

For example, when a key is accepted for a token as `KindeClientSDK.get_claim(conn, "jti", :id_token)` the return claim value would be as follows.

```elixir
"57012a4-82ca-41f-8f19-0a5c3cc653
```

## **Permissions**

When a user signs in to an organization the access token your product/application contains a custom claim with an array of permissions for that user.

You can set permissions in your Kinde account. Here’s an example.

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

For more details See [Define user permissions](/manage-users/roles-and-permissions/user-permissions/).

We provide helper functions to more easily access permissions.

```elixir
KindeClientSDK.get_permissions(conn)
KindeClientSDK.get_permission(conn, "create:todos")
```

`get_permission` checks the permission value and returns if it is granted or not (i.e. checks if permission key exists in the `permissions` claim array) and checks the relevant org code by checking against claim `org_code`.

## **Audience**

An `audience` is the intended recipient of an access token - for example the API for your application. The audience argument can be passed to the Kinde client to request an audience be added to the provided token.

```elixir
additional_params = %{
      audience: "api.yourapp.com"
    }

KindeClientSDK.init(
  conn,
  Application.get_env(:kinde_sdk, :domain),
  Application.get_env(:kinde_sdk, :redirect_url),
  Application.get_env(:kinde_sdk, :backend_client_id),
  Application.get_env(:kinde_sdk, :client_secret),
  :authorization_code_flow_pkce,
  Application.get_env(:kinde_sdk, :logout_redirect_url),
  "openid profile email offline",
  additional_params
  )
```

For details on how to connect, see [Register an API](/developer-tools/your-apis/register-manage-apis/).

## **Overriding scope**

By default the KindeSDK requests the following scopes:

- profile
- email
- offline
- openid

You can override this by passing scope into the KindeSDK.

## **Persisting authentication state on page refresh or new tab**

When a user refreshes the page or opens a new tab, the authentication state can be lost. To work around this issue, there are two possible solutions:

- Use cookies to store the authentication token. This can be done by setting an `httpOnly` cookie with the authentication token, which will be sent to the server with every request, allowing the server to maintain the authentication state.
- Use a session store to store the authentication token. Elixir has several session store options available, including using a database, in-memory cache, or distributed cache.

Once one of these solutions is implemented, there is no need for additional action to persist the authentication state.

## **Token storage**

Once the user has successfully authenticated, you’ll have a JWT and possibly a refresh token that should be stored securely.

## API reference - C**reate Kinde Client**

### `domain`

Either your Kinde URL or your custom domain. e.g `https://yourapp.kinde.com`

Type: `string`

Required: Yes

### `redirect_url`

The URL that the user will be returned to after authentication.

Type: `string`

Required: Yes

### `backend_client_id`

The unique ID of your backend application in Kinde.

Type: `string`

Required: Yes

### `frontend_client_id`

The unique ID of your frontend application in Kinde.

Type: `string`

Required: Yes

### `client_secret`

The unique client secret associated with your application in Kinde.

Type: `string`

Required: No

### `logout_redirect_url`

Where your user will be redirected upon logout.

Type: `string`

Required: No, except for PKCE flow

### `scope`

The scopes to be requested from Kinde.

Type: `string`

Required: No

Default: `openid profile email offline`

### `additional_parameters`

Additional parameters that will be passed in the authorization request.

Type: `map`

Required: No

Default: `%{}`

### `additional_parameters_audience`

The audience claim for the JWT.

Type: `string`

Required: No

### `additional_parameters_org_name`

The org claim for the JWT.

Type: `string`

Required: No

### `additional_parameters_org_code`

The org claim for the JWT.

Type: `string`

Required: No

## API reference - Kinde Client **Functions**

### `login`

Constructs a redirect URL and sends the user to Kinde to sign in.

Arguments: `conn, client`

Usage:

```elixir
KindeClientSDK.login(conn, client)
```

Sample output: `redirect`

### `register`

Constructs a redirect URL and sends the user to Kinde to sign up.

Arguments: `conn, client`

Usage:

```elixir
KindeClientSDK.register(conn, client)
```

Sample: `redirect`

### `logout`

Logs the user out of Kinde.

Arguments: `conn`

Usage:

```elixir
KindeClientSDK.logout(conn)
```

Sample: `redirect`

### `get_token`

Returns the raw access token from URL after logged in from Kinde.

Arguments: `conn`

Usage:

```elixir
KindeClientSDK.get_token(conn)
```

Sample:

```elixir
eyJhbGciOiJIUzI1...
```

### `create_org`

Constructs the redirect URL to sign up and create a new organization in your business.

Arguments: `conn`, `atom`

Usage:

```elixir
KindeClientSDK.create_org(conn, client)
```

Sample: `redirect`

### `get_claim`

Gets a claim from an access or ID token.

Arguments: `conn, string, atom`

Usage:

```elixir
KindeClientSDK.get_claim(conn, "jti") or KindeClientSDK.get_claim(conn, "jti", :id_token)
```

Sample:

```elixir
%{name: "iss", value: "https://elixirsdk2.kinde.com"}
```

### `get_claims`

Gets all claims from an access or ID token.

Arguments: `conn, atom`

Usage:

```elixir
KindeClientSDK.get_claims(conn)
 or 
KindeClientSDK.get_claims(conn, :id_token)
```

Sample:

```elixir
%{"aud" => [], "azp" => "", ...}
```

### `get_permission`

Returns the state of a given permission.

Arguments: `conn, string`

Usage:

```elixir
KindeClientSDK.get_permission(conn, "create:users")
```

Sample:

```elixir
%{org_code: 'org_1234', is_granted: true}
```

### `get_permissions`

Returns all permissions for the current user for the organization they are signed into.

Arguments: `conn, atom`

Usage:

```elixir
KindeClientSDK.get_permissions(conn, :id_token)
```

Sample:

```elixir
%{org_code: 'org_1234', permissions: ['create:todos', 'update:todos', 'read:todos']}
```

### `get_organization`

Get details for the organization your user is signed into.

Arguments: `conn`

Usage:

```elixir
KindeClientSDK.get_user_organization(conn)
```

Sample:

```elixir
%{org_code: "org_9d78"}
```

### `get_organizations`

Gets an array of all organizations the user has access to.

Arguments: `conn`

Usage:

```elixir
KindeClientSDK.get_user_organizations(conn)
```

Sample:

```elixir
%{org_codes: ["org_9d78", "org_aca6c", "org_27e56"]}
```

### `get_user_details`

Returns the profile for the current user.

Arguments: `conn`

Usage:

```elixir
KindeClientSDK.get_user_detail(conn)
```

Sample:

```elixir
%{
email: "dev@smit.com", family_name: "Smith", given_name: "Dave", id: "kp:abcdef"
}
```

### `get_cache_pid`

Returns the Kinde cache PID from the `conn`.

Arguments: `conn`

Usage:

```elixir
KindeClientSDK.get_cache_pid(conn)
```

Sample:

```elixir
#PID
```

### `save_kinde_client`

Saves the Kinde client created into the `conn`.

Arguments: `conn`

Usage:

```elixir
KindeClientSDK.save_kinde_client(conn)
```

Sample:

```elixir
:ok
```

### `get_kinde_client`

Returns the Kinde client created from the `conn`.

Arguments: `conn`

Usage:

```elixir
KindeClientSDK.get_kinde_client(conn)
```

Sample:

```elixir
%KindeClientSDK{
cache_pid: #PID<0.123.0>,
domain: "abcd.com",
redirect_uri: "…/callback",
logout_redirect_uri: …
}
```

### `get_all_data`

Returns all the Kinde data (tokens) returned.

Arguments: `conn`

Usage:

```elixir
KindeClientSDK.get_all_data(conn)
```

Sample:

```elixir
%{
access_token: "eyJhbGciOiJSU…”,"
expires_in: 1234,
id_token: “abcdedhjshfsjg”,…
}
```

## **Feature flag helper functions**

### `get_flag/2`

Details of a feature-flag.

Arguments: `conn, code`

Usage:

```elixir
KindeClientSDK.get_flag(conn, code)
```

Sample output:

```elixir
flag: %{
"code" => "theme",
"is_default" => false,
"type" => "string",
"value" => "grayscale
}
```

### `get_flag/3`

The default value of a feature flag.

Arguments: `conn, code`, `default value`

Usage:

```elixir
KindeClientSDK.get_flag(conn, code, default_value)
```

Sample output:

```elixir
flag: %{
"code" =>
"create_competition",
"is_default" => true, "value" => false
}
```

### `get_flag/4`

The type and default value of a feature flag.

Arguments: `conn, code, default value, flag_type`

Usage:

```elixir
KindeClientSDK.get_flag(conn, code, default_value, flag_type)
```

Sample output:

```elixir
flag: %{
"code" =>
"theme",
"is_default" => true,
"value" => "black
}
```

### `get_boolean_flag/2`

Returns the boolean flag.

Arguments: `conn, code`

Usage:

```elixir
KindeClientSDK.get_boolean_flag(conn, code)
```

Sample output:

```elixir
**true** | **false** |
flag: "Error e.g flag does not exist and no default provided”"
```

### `get_boolean_flag/3`

Returns the boolean flag value.

Arguments: `conn, code, default value`

Usage:

```elixir
KindeClientSDK.get_boolean_flag(conn, code, default_value)
```

Sample output:

```elixir
flag: false
```

### `get_string_flag/2`

Returns the string flag value.

Arguments: `conn, code`

Usage:

```elixir
KindeClientSDK.get_string_flag(conn, code)
```

Sample output: corresponding values from object or error-messages

### `get_string_flag/3`

Returns the string flag value.

Arguments: `conn, code, default value`

Usage:

```elixir
KindeClientSDK.get_string_flag(conn, code, default_value)
```

Sample output:

```elixir
flag: "black”"
```

### `get_integer_flag/2`

Returns the integer flag value.

Arguments: `conn, code`

Usage:

```elixir
KindeClientSDK.get_integer_flag(conn, code)
```

Sample output: corresponding values from object or error-messages

### `get_integer_flag/3`

Returns the integer flag value.

Arguments: `conn, code, default value`

Usage:

```elixir
KindeClientSDK.get_integer_flag(conn, code, default_value)
```

Sample output:

```elixir
flag: 46
```

If you need help connecting to Kinde, please contact us at [support@kinde.com](mailto:support@kinde.com).
