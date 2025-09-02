
<Aside type="warning">

This SDK has been superseded by a [new version](/developer-tools/sdks/backend/python-sdk/).

</Aside>

The Kinde Python SDK allows developers to quickly and securely integrate a new or an existing Python application into the Kinde platform.

## Before you begin

- Kinde Python SDK supports Python 3.8+
- If you haven’t already got a Kinde account, [register for free here](https://app.kinde.com/register) (no credit card required). Registering gives you a Kinde domain, which you need to get started, e.g. `yourapp.kinde.com`.

For new projects, you can also find our [Starter Kit on GitHub](https://github.com/kinde-starter-kits/python-starter-kit).

## Install

Install [PIP](https://pip.pypa.io/en/stable/installation/) and then execute the following command:

```bash
pip install kinde-python-sdk
```

### Set callback URLs

1. In Kinde, go to **Settings > Applications > [Your app] > View details**.
2. Add your callback URLs in the relevant fields. For example:
   - Allowed callback URLs (also known as redirect URIs) - for example, `http://localhost:8000/callback`
   - Allowed logout redirect URLs - for example, `http://localhost:8000`
3. Select **Save**.

### Add environments

Kinde comes with a production environment, but you can set up other environments if you want to. Note that each environment needs to be set up independently, so you need to use the Environment subdomain in the code block above for those new environments.

## Configure your app

**Environment variables**

The following variables need to be replaced in the code snippets below.

- `KINDE_HOST` - your Kinde domain, e.g. `https://yourdomain.kinde.com`
- `KINDE_CLIENT_ID` - In Kinde, go to **Settings > Applications > [your application] > View details**.
- `KINDE_CLIENT_SECRET` - In Kinde, go to **Settings > Applications > [your application] > View details**.
- `KINDE_REDIRECT_URL` - your callback urls or redirect URIs, e.g. `http://localhost:8000/callback`
- `KINDE_POST_LOGOUT_REDIRECT_URL` - where you want users to be redirected to after signing out, e.g. `http://localhost:8000`

## Integrate with your app

Create a new instance of the Kinde Auth client object before you initialize your app.

```python
...
from kinde_sdk import Configuration
from kinde_sdk.kinde_api_client import GrantType, KindeApiClient
...

configuration = Configuration(host=KINDE_HOST)
kinde_api_client_params = {
    "configuration": configuration,
    "domain": KINDE_HOST,
    "client_id": KINDE_CLIENT_ID,
    "client_secret": KINDE_CLIENT_SECRET,
    "grant_type": GRANT_TYPE, # client_credentials | authorization_code | authorization_code_with_pkce
    "callback_url": KINDE_REDIRECT_URL
}
kinde_client = KindeApiClient(**kinde_api_client_params)
```

With **PKCE** flow, the `code_verifier` is required.

```python
from authlib.common.security import generate_token
CODE_VERIFIER = generate_token(48)
kinde_api_client_params["code_verifier"] = CODE_VERIFIER
```

## Sign in and sign up

The Kinde client provides methods for easy sign in and sign up. You can add buttons in your HTML as follows:

```html
<div class="navigation">
  <a href="{{ url_for('login') }}" type="button">Sign in</a>
  <a href="{{ url_for('register') }}" type="button">Sign up</a>
</div>
```

You will also need to route `/login` and `/register` to the SDK methods:

```python
@app.route("/login")
def login():
    return app.redirect(kinde_client.get_login_url())


@app.route("/register")
def register():
    return app.redirect(kinde_client.get_register_url())
```

## Manage redirects

When the user is redirected back to your site from Kinde, this will call your callback URL defined in the `KINDE_REDIRECT_URL` variable. You will need to route `/callback` to call a function to handle this.

```python
@app.route("/callback")
def callback():
    kinde_client.fetch_token(authorization_response=request.url)
    print(configuration.access_token) # Token here
```

The code above setups up the Kinde client as a single instance per user session. If you want to setup the Kinde client as a singleton, you can do the following:
```python
@app.route("/callback")
def callback():
    access_token: dict = kinde_client.fetch_token_value(authorization_response=request.url)
    print(access_token) # Token here
```
This logic leaves the Kinde client responsible for managing the access_token, but not for storing them. This is left up to the developer to implement.

You can also get the current authentication status with `is_authenticated`.

```python
if kinde_client.is_authenticated():
		# Core here
```
The code above will check if a user is authenticated by checking its internal state. The down side is that the there will have to be an instance of the Kinde client for each user session. This is costly and not scalable.

```python
if kinde_client.is_authenticated_token(access_token):
		# Core here
```


**Note:** The kinde_client object that is created stores the access_token. This means you need to create a kinde_client object for each unique user that is signing in to your application, so that you can keep track of whether the user is authenticated or not.

## Logout

The SDK comes with a logout method that returns a logout URL.

```python
kinde_client.logout(redirect_to=KINDE_POST_LOGOUT_REDIRECT_URL)

@app.route("/logout")
def logout():
    return app.redirect(
        kinde_client.logout(redirect_to=KINDE_POST_LOGOUT_REDIRECT_URL)
    )
```

## Get user information

To access the user information, use the `get_user_details` helper function:

```python
kinde_client.get_user_details();
{
	"given_name":"Dave",
	"id":"abcdef",
	"family_name":"Smith",
	"email":"dave@smith.com",
	"picture": "https://link_to_avatar_url.kinde.com"
}
```

### View users in Kinde

Go to the **Users** page in Kinde to see who has registered.

## User permissions

After a user signs in and they are verified, the token return includes permissions for that user. [User permissions are set in Kinde](/manage-users/roles-and-permissions/user-permissions/), but you must also configure your application to unlock these functions.

```python
permissions = [
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

```python
kinde_client.get_permission("create:todos")

kinde_client.get_permissions()
```

A practical example in code might look something like:

```python
-if kinde_client.get_permission("create:todos").get("is_granted")):
+if kinde_client.get_permission("create:todos").get("is_granted"):

The code above will check against a session managed through the Kinde client. If you want to check against a specific access token, you can do the following:

```python
permission: dict = kinde_client.get_permission_token(access_token, "create:todos")
```

## Feature Flags

We have provided a helper to grab any feature flag from `access_token`:

```python
kinde_client.get_flag("theme");
{
    "code": "theme",
    "type": "string",
    "value": "pink",
    "is_default": False # whether the fallback value had to be used
}

kinde_client.get_flag("is_dark_mode");
{
    "code": "is_dark_mode",
    "type": "boolean",
    "value": True,
    "is_default": False
}

kinde_client.get_flag("create_competition", default_value = False);
{
    "code": "create_competition",
    "type" => "boolean",
    "value": False,
    "is_default": True # because fallback value had to be used
}

kinde_client.get_flag("competitions_limit", default_value = 3, flat_type = "s");


kinde_client.get_flag("new_feature");

# this will return the flag value for the given access token
kinde_client.get_flag_token(access_token, "new_feature");
```

We also provide wrapper functions which should leverage `getFlag` above.

**Get boolean flags**

```python
kinde_client.get_boolean_flag("is_dark_mode");

kinde_client.get_boolean_flag("is_dark_mode", False);

kinde_client.get_boolean_flag("new_feature", False);

kinde_client.get_boolean_flag("new_feature");

kinde_client.get_boolean_flag("theme", False);

kinde_client.get_boolean_flag_token(access_token, "new_feature");
```

**Get string flags**

```python
kinde_client.get_string_flag("theme");

kinde_client.get_string_flag("theme", False);

kinde_client.get_string_flag("cta_color", False);

kinde_client.get_string_flag("cta_color");

kinde_client.get_string_flag("is_dark_mode", False);

kinde_client.get_string_flag_token(access_token, "cta_color");
```

**Get integer flags**

```python
kinde_client.get_integer_flag("competitions_limit");

kinde_client.get_integer_flag("competitions_limit", 3);

kinde_client.get_integer_flag("team_count", 2);

kinde_client.get_integer_flag("team_count");

kinde_client.get_integer_flag("is_dark_mode", False);

kinde_client.get_integer_flag_token(access_token, "team_count");
```

## Audience

An `audience` is the intended recipient of an access token - for example the API for your application. The audience argument can be passed to the Kinde client to request an audience be added to the provided token.

The audience of a token is the intended recipient of the token.

```python
kinde_api_client_params["audience"] = "api.yourapp.com
```

For details on how to connect, see [Register an API](/developer-tools/your-apis/register-manage-apis/)

## Overriding scope

By default the `KindeSDK` requests the following scopes:

- profile
- email
- offline
- openid

You can override this by passing scope into the `KindeSDK`.

```python
kinde_api_client_params["scope"] = "profile email offline openid
```

### Getting claims

We have provided a helper to grab any claim from your id or access tokens. The helper defaults to access tokens:

```python
kinde_client.get_claim("aud")

kinde_client.get_claim("given_name", "id_token")

kinde_client.get_claim_token(access_token, "given_name")
```

## Organizations

### Create an organization

To create a new organization within your application, you will need to run a similar function to below:

```python
return app.redirect(kinde_client.create_org())
```

### Sign up and sign in to organizations

Kinde has a unique code for every organization. You’ll have to pass this code through when you register a new user or sign in to a particular organization. Example function below:

```python
kinde_api_client_params["org_code"] = 'your_org_code'

@app.route("/login")
def login():
    return app.redirect(kinde_client.get_login_url())


@app.route("/register")
def register():
    return app.redirect(kinde_client.get_register_url())
```

Following authentication, Kinde provides a json web token (jwt) to your application. Along with the standard information we also include the `org_code` and the permissions for that organization (this is important as a user can belong to multiple organizations and have different permissions for each).

Example of a returned token:

```python
{
   "aud": [],
   "exp": 1658475930,
   "iat": 1658472329,
   "iss": "https://your_subdomain.kinde.com",
   "jti": "123457890",
   "org_code": "org_1234",
   "permissions": ["read:todos", "create:todos"],
   "scp": [
		   "openid",
		   "profile",
		   "email",
		   "offline
   ],
   "sub": "kp:123457890"
}
```

The `id_token` will also contain an array of organizations that a user belongs to - this is useful if you wanted to build out an organization switcher for example.

```python
{
		...
		"org_codes": ["org_1234", "org_4567"],
		...
};
```

There are two helper functions you can use to extract information:

```python
kinde_client.get_organization()

kinde_client.get_user_organizations()
```

For more information about how organizations work in Kinde, see [Kinde organizations for developers](/build/organizations/orgs-for-developers/).

### Token storage

Once the user has successfully authenticated, you'll get a JWT and possibly a refresh token that should be stored securely.

## SDK API reference

### `domain`

Either your Kinde instance url or your custom domain. e.g. `https://yourapp.kinde.com`.

Type: `string`

Required: Yes

### `callback_url`

The url that the user will be returned to after authentication.

Type: `string`

Required: Yes

### `client_id`

The ID of your application in Kinde.

Type: `string`

Required: Yes

### `grant_type`

Define the grant type when using the SDK.

Type: `string`

Required: Yes

### `client_secret`

The unique client secret associated with your application in Kinde.

Type: `string`

Required: No

### `code_verifier`

PKCE works by having the app generate a random value at the beginning of the flow called a Code Verifier.

Type: `string`

Required: No, except for PKCE flow

### `scope`

The scopes to be requested from Kinde.

Type: `string`

Required: No

Default: `openid profile email offline`

### `audience`

The audience claim for the JWT.

Type: `string`

Required: No

### `org_code`

Additional parameters that will be passed in the authorization request.

Type: `string`

Required: No

## KindeSDK methods

### `get_login_url`

Constructs a redirect URL and sends the user to Kinde to sign in. Optional parameters are used for custom sign-up and sign-in and are documented [here](/authenticate/custom-configurations/custom-authentication-pages/).

Arguments (optional):

```python
auth_url_params: Optional[Dict[str, Dict[str, str]]]
```

Usage:

```python
kinde_client.get_login_url()

kinde_client.get_login_url({
		"auth_url_params": {
			"connection_id": "conn_6a95dec504d34dc286dc80e8df9f6099"
		}
})


```

Sample output:

```python
https://your_host.kinde.com/oauth2/auth?response_type=code&…
```

### `get_register_url`

Constructs a redirect URL and sends the user to Kinde to sign up. Optional parameters are used for custom sign-up and sign-in and are documented [here](/authenticate/custom-configurations/custom-authentication-pages/).

Arguments (optional):

```python
auth_url_params: Optional[Dict[str, Dict[str, str]]]
```

Usage:

```python
kinde_client.get_register_url()

kinde_client.get_register_url({
		"auth_url_params": {
			"connection_id": "conn_6a95dec504d34dc286dc80e8df9f6099"
		}
})
```

Sample:

```python
https://your_host.kinde.com/oauth2/auth?response_type=code&…
```

### `logout`

Logs the user out of Kinde.

Arguments:

```python
redirect_to: str
```

Usage:

```python
kinde_client.logout(redirect_to="KINDE_POST_LOGOUT_REDIRECT_URL")
```

Sample:

```python
https://your_host.kinde.com/logout?redirect=https://…
```

### `fetch_token`

Returns the raw access token from URL after logged in from Kinde.

Arguments:

```python
authorization_response: str
```

Usage:

```python
kinde_client.fetch_token(authorization_response=”[http://localhost:8000?code=42..e9&state=d..t](https://example.com/github?code=42..e9&state=d..t)”)

token: dict = kinde_client.fetch_token_value(authorization_response=”[http://localhost:8000?code=42..e9&state=d..t](https://example.com/github?code=42..e9&state=d..t)”)
```

Sample:

```python
eyJhbGciOiJIUzI1...
```

### `refresh_token`

Get new access token from Kinde if existed `refresh_token`.

Usage:

```python
kinde_client.refresh_token()

kinde_client._refresh_token_value(refresh_value)
```

### `create_org`

Return the redirect URL to sign up and create a new organization in your business.

Usage:

```python
kinde_client.create_org()
```

Sample:

```python
https://your_host.kinde.com/oauth2/auth?response_type=code&…
```

### `get_claim`

Gets a claim from an access or ID token.

Arguments:

```python
claim: str, token_name?: str # default: access_token
```

Usage:

```python
kinde_client.get_claim("given_name", "id_token")

kinde_client.get_claim_token(access_token, "given_name")
```

Sample:

```python
{"name": "given_name", "value": "David"}
```

### `get_permission`

Returns the state of a given permission.

Arguments:

```python
key: str
```

Usage:

```python
kinde_client.get_permission(”read:todos”)

kinde_client.get_permission_token(access_token, "read:todos")
```

Sample:

```python
{”org_code”: "org_b235c067b7e4", is_granted: True}
```

### `get_permissions`

Returns all permissions for the current user for the organization they are signed into.

Usage:

```python
kinde_client.get_permissions()

kinde_client.get_permissions_token(access_token)
```

Sample:

```python
{"org_code": "org_b235c067b7e4", permissions: [ "create:users", "view:users" ]}
```

### `get_organization`

Get details for the organization your user is signed into.

Usage:

```python
kinde_client.get_organization()

kinde_client.get_organization_token(access_token)
```

Sample:

```python
{"org_code": "org_1234"}
```

### `get_organizations`

Gets an array of all organizations the user has access to.

Usage:

```python
kinde_client.get_user_organizations()

kinde_client.get_user_organizations_token(access_token)
```

Sample:

```python
{"org_codes": ["org_1234", "org_abcd"]}
```

### `get_user_details`

Returns the profile for the current user.

Usage:

```python
kinde_client.get_user_details()

kinde_client.get_user_details_token(access_token)
```

Sample:

```python
{
	"given_name":"Dave",
	"id":"abcdef",
	"family_name":"Smith",
	"email":"dave@smith.com",
	"picture": "https://link_to_avatar_url.abc.com"
}
```

### `get_flag`

Gets a feature flag from an access token.

Arguments:

```python
code: str
default_value?: str
flag_type?: str
```

Usage:

```python
kinde_client.get_flag("theme");

kinde_client.get_flag_token(access_token, "theme");
```

Sample:

```python
{
    "code": "theme",
    "type": "string",
    "value": "pink",
    "is_default": False
}
```

### `get_boolean_flag`

Gets a boolean feature flag from an access token.

Arguments:

```python
code: str
default_value?: str
```

Usage:

```python
kinde_client.get_boolean_flag("is_dark_mode");

kinde_client.get_boolean_flag_token(access_token, "is_dark_mode");
```

Sample: `True` or `False`

### `get_string_flag`

Gets a string feature flag from an access token.

Arguments:

```python
code: str
default_value?: str
```

Usage:

```python
kinde_client.get_string_flag("theme");

kinde_client.get_string_flag_token(access_token, "theme");
```

Sample: `“pink”`

### `get_integer_flag`

Gets a integer feature flag from an access token

Arguments:

```python
code: str
default_value?: str
```

Usage:

```python
kinde_client.get_integer_flag("competitions_limit");

kinde_client.get_integer_flag_token(access_token, "competitions_limit");
```

Sample: `5`

### `is_authenticated()`

To check user authenticated or not.

Sample: `true` or `false`

If you need help connecting to Kinde, please contact us at [support@kinde.com](mailto:support@kinde.com).
