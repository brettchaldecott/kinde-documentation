
You can pass provider-specific parameters to an Identity Provider (IdP) during authentication. These are also known as 'upstream params'. The values your pass can either be static per connection or dynamic per user.

There's a number of reason why you might want to use upstream params:

- to create a smoother sign in experience - by passing the email through
- to offer an account switcher (such as the Google account switcher) during sign in

Upstream params are available for OAuth 2.0 connections, e.g. [social connections](/authenticate/social-sign-in/add-social-sign-in/), [Entra ID OAuth 2.0 enterprise connection](/authenticate/enterprise-connections/azure/), and as part of [advanced configurations](/authenticate/enterprise-connections/advanced-saml-configurations/) in SAML connections.

## Limitations

Every identity provider has their own set of supported parameters and values, so you'll need to check their documentation to determine which URL parameters are supported.

## Static parameters

Static parameters can be useful when you have specific values you always want to pass on to the IDP. These are set in the connecction configuration screen.

![Screen shot of google connection screen and upstream params field](https://imagedelivery.net/skPPZTHzSlcslvHjesZQcQ/3fa86bd8-9005-4022-7118-fc93ce4f4a00/public)

The Upstream parameter field accepts JSON and the structure is as follows:

```json
{
  "<param_name_to_pass>": {
    "value": "<your_hardcoded_value>"
  }
}
```

Replace `<param_name_to_pass>` with the name of the parameter you wish to pass upstream to the IDP.
Replace `<your_hardcoded_value>` with the value of the parameter you wish to pass upstream.

### Example: Force the google account selector to display on sign in

If you want Google to always show the account selector even if the user is already logged in with a Google account, pass the `prompt=select_account` parameter from Kinde.
This is how that would look:

```json
{
  "prompt": {
    "value": "select_account"
  }
}
```

Now, when your user clicks on the Google button and Kinde creates the URL to redirect to Google, it will append`&prompt=select_account`.

## Dynamic parameters

Dynamic parameters cover the case where you don't know the value of the parameter ahead of time, and it needs to be populated on the fly during the auth flow. For example, if you need to pass on a parameter that was provided to Kinde in the auth URL.

This is the structure.

```json
{
  "<param_name_to_pass>": {
    "alias": "<dynamic_param_name>"
  }
}
```

The `alias` keyword tells Kinde which parameter from your auth url to use, and the value to pass upstream to the IDP.

Here is an example where we provide `login_hint` as part of the auth URL, where the email [`&login_hint=hello@example.com`](mailto:&login_hint=hello@example.com) is included on the URL.

```html
https://<your_kinde_subdomain
  >.kinde.com/oauth2/auth ?response_type=code &client_id=<your_kinde_client_id>
    &redirect_uri=<your_app_redirect_url>
      &scope=openid%20profile%20email &state=abc
      &login_hint=hello@example.com</your_app_redirect_url
    ></your_kinde_client_id
  ></your_kinde_subdomain
>
```

In this case both Kinde and the IDP use the parameter name `login_hint` so the configuration is the same on both sides:
Add this to the connection configuration:

}
}

````

In this case we are saying pass the `login_hint` parameter upstream to the IDP with the value Kinde received in the `login_hint` auth url param. So `&login_hint=hello@example.com` would be passed on to the provider.

Where the `alias` becomes especially powerful is when you want to re-map a parameter name to match the one an IDP expects. For example, let’s say that our IDP expects `username`  instead of `login_hint` for the same value, in this case our JSON would look like this:

```json
{
	"username": {
		"alias": "login_hint"
	}
}
````

In this case we are saying pass the `username` parameter upstream to the IDP with the value Kinde received in the `login_hint` auth url param. We remap the email value from `login_hint` to `username` and the parameter `&username=hello@example.com` would be passed on to the IDP.

## Kinde-provided aliases

When an email address is populated during the auth flow, we make this available via the `login_hint` alias.

You might use this if you are using Home realm discovery with an Entra ID OAuth2.0 connection, and you want to pass the URL that the user entered on Kinde as the `login_hint`, upstream to Entra, to prevent the user having to enter their email twice.

If the user enters `hello@example.com` in the Kinde email field with the following configuration active, we set the `login_hint` parameter to `hello@example.com` via the Kinde provided alias.

```json
{
  "login_hint": {
    "alias": "login_hint"
  }
}
```

## Multiple parameters

You can send multple parameters this way and mix-and-match between dynamic and static in the same configuration. For example if the user entered `hello@example.com` and the following was configured:

```json
{
  "prompt": {
    "value": "login"
  },
  "username": {
    "alias": "login_hint"
  }
}
```

This would result in `&prompt=login&username=hello@example.com`

## Supported aliases

The values which can be used as an `alias` are:

- `prompt`
- `login_hint`

If you need other aliases added, let us know via a [feedback form](https://updates.kinde.com/).
