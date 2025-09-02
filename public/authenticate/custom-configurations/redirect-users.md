
After authenticating a user in Kinde, you can return them to a specific page within your application.

Users are initially redirected back to the requested [Callback url](/get-started/connect/callback-urls/) you have included in your allowlist within Kinde. This is necessary to complete the token exchange and finalize the authentication flow.

## When to set a specific redirect

A callback URL is not always where you want users redirected after authentication. You may want users to land on a specific dashboard page, or to trigger authentication if a user tries to access a protected page in your application.

In both those cases, you can store a URL to redirect the user back to their intended page (after authentication) to provide a more seamless experience.

## Redirect without an SDK

Most of our SDKs include a mechanism for redirecting users. However, if you are not using a Kinde SDK, use one of the following methods.

1. Store the intended URL in a cookie or local storage.
2. Use the `state` parameter.

## Store the URL in a cookie or local storage

For single page applications the simplest is probably to leverage local storage to store the desired URL.

Prior to redirecting to Kinde:

```jsx
localStorage.setItem("nextUrl", "/some-protected-route");
```

After authentication is complete:

```jsx
const nextUrl = localStorage.getItem("nextUrl");
window.location.replace(nextUrl);
```

For server-side applications you can achieve the same thing with a cookie - essentially setting the next URL before redirecting to Kinde and fetching the value post-authentication. The implementation will depend on your language or framework choice.

## Use the state parameter

You should be using the state parameter already to protect against CSRF attacks. [(Here's how Kinde uses the State param)](/get-started/learn-about-kinde/kinde-product-security/#csrf-protections-via-state-parameter). Essentially it's a random string that you would store in your application, so when you receive the response from Kinde you can validate it matches the one you sent.

Because it is just a string, you can leverage it to store additional information, like the intended destination of your user.

1. Generate a random string in your application. For this example we will use:`BlueFox0101`.
2. Use this string as key for an object with the value of your application state and store this locally. For example:

   ```jsx
   {
   	"BlueFox0101" : {
   			nextUrl: '/some-protected-route',
   	}
   }
   ```

3. When you redirect your user to Kinde to complete the authentication flow, include the random string as the `state` param:

   ```jsx
   https://<your_kinde_subdomain>.kinde.com/oauth2/auth
   	?response_type=code
   	&client_id=<your_kinde_client_id>
   	&redirect_uri=<your_app_redirect_url>
   	&scope=openid%20profile%20email
   	&state=BlueFox0101
   ```

4. After the user has authenticated, they will be redirected back to your application and the `state` value will be included in the url:

   ```jsx
   https://<your_application>.com/auth/callback
       ?code=<some_unique_code>
       &scope=openid%20profile%20email
       &state=BlueFox0101
   ```

5. As part of your callback processing and response validation, verify that the `state` returned in the URL above matches the random string you stored locally. If it does, retrieve the rest of the application state (like the nextUrl).
6. Use the `code` param to complete the token exchange (as per the [Use Kinde without an SDK](/developer-tools/about/using-kinde-without-an-sdk/#handling-the-callback) guide) and once the exchange is complete use the `nextUrl` to redirect the user.

## **Limitations and considerations**

- Choose a storage method based on your application type.

  | App Type        | Recommended storage |
  | --------------- | ------------------- |
  | Regular Web App | Cookie or session   |
  | SPA             | Local browser       |
  | Native App      | Memory or local     |

- `State` parameter values are not unlimited. `414 Request-URI Too Large` means you should try a smaller value.
- Passing URLs in plain text or in any predictable way is unsafe. Ensure that the `state` parameter value is unique and opaque to ensure that it can be used for defence against CSRF and phishing attacks.
- If the `state` parameter value is stored in a cookie, it should be signed to prevent forgery.

## A secure way to store redirect information

The `state` parameter can mitigate [**CSRF attacks**](https://en.wikipedia.org/wiki/Cross-site_request_forgery) by using a unique and non-guessable value associated with each authentication request about to be initiated. That non-guessable value allows you to prevent the attack by confirming that the value coming from the response, matches the one you sent.

The `state` parameter is also a string, so you can encode any information in it. You can send a random value when starting an authentication request and validate the received value when processing the response. You store something on the client application side (in cookies, session, or local storage) that allows you to perform the validation.

Kinde SDKs handle `state` generation and validation automatically.
