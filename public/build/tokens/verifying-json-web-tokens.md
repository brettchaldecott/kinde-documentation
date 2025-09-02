
Kinde uses JSON Web Tokens (JWT) for secure data transmission, authentication, and authorization.

JWT verification ensures that only authorized users and apps can access your regular web, native, or single-page applications, by checking that tokens are valid, and have not been tampered with, misused, or are expired.

The validation process checks the structure, claims, and signature of the JWT.

If you are [setting up with Kinde without an SDK](/developer-tools/about/using-kinde-without-an-sdk/), or if you are using a mobile or front-end SDK and want to protect your back-end APIs, this topic is relevant for you.

## **How the JWTs work**

### **Signing algorithm**

The JWT signature is generated using a ’signing algorithm’. Kinde supports:

- RSA with 2048-bit key
- SHA-256 hashing algorithm
- RSA signature with SHA-256

### **Hash values**

When validating a JWT, generally, the current hash value and the original hash value are parsed, or decoded, then compared to verify the token signature is authentic. This is part of token encryption.

## **Methods to verify JWTs**

If you are not using one of our SDKs, you can parse and validate a JWT by:

- Using any existing middleware for your web framework.
- Choosing a third-party library, for example the OpenID Foundation has [a list of libraries for working with JWT tokens](https://openid.net/developers/jwt/)

See also [Kinde’s supported languages and frameworks](/developer-tools/about/our-sdks/).

## **Asymmetric signing algorithm (RSA)**

Verify that the token is signed with `RS256` algorithm (see the `alg` header in the token response). Kinde only supports signing tokens with the asymmetric signing algorithm (RSA). We don’t support `HMAC` signing by design.

## **JSON Web Key**

It’s likely you will be using a library to validate your JWTs and they will require the url for your public JSON Web Key (also known as a `jwks` file).

The file can be found here:

`https://<your_subdomain>.kinde.com/.well-known/jwks`

## **Included in the Kinde access token**

### **Issuer (iss) claim**

Verify the `iss` claim, that the token was issued by your Kinde environment. Each environment has a unique `iss` claim.

### **Audience (aud) claims**

If you are authenticating an API, verify `aud` claims in the token. We support multiple `aud` claims which are passed in the token as a JSON array.

### **State versus stateless**

For increased security in a back-end application, verify the `state` that you provided in the callback. Deny all requests with a `state` that your application does not recognize.

You can use `state` for front-end applications, but it does not increase security.

## The getToken function

The `getToken` function stores an in-memory cache of the access token, which it returns by default. If the token is about to expire it will use a refresh token to get a new access token from Kinde silently in the background so additional network requests to Kinde are only made when absolutely necessary.

To keep tokens secure, they should only be stored in the back end of your application. Tokens become unsecured if stored in a browser’s local storage, indexed database, or session storage.
