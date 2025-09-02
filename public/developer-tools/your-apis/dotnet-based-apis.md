
{/* @case-police-ignore Uri */}
{/* @case-police-ignore Jwt */}

This article walks through configuring an ASP.NET application to accept and validate an access token from Kinde.

Where users are logged into a front end or mobile application, the access token can be passed in a bearer authorization header to secure requests to back end APIs.

A complete sample project can be found in the .NET [starter kit](https://github.com/kinde-starter-kits/dotnet-webapi-starter-kit).

## Configure your project

1. Install the packages that handle tokens.

   ```bash
   dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
   dotnet add package Microsoft.IdentityModel.Protocols.OpenIdConnect
   ```

2. Configure application services (typically `Program.cs`) to use the package.

   ```csharp
   builder.Services
       .AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
       .AddJwtBearer(options =>
       {
         // These two lines map the Kinde user ID to Identity.Name (optional).
         options.MapInboundClaims = false;
         options.TokenValidationParameters.NameClaimType = "sub";
       });
   ```

3. Configure `appsettings.json` for your Kinde application. Replace `<your-domain>` with your Kinde domain and `<your-audience>` with the audience associated with your API in Kinde.

   ```json
     "Authentication": {
       "Schemes": {
         "Bearer": {
           "Authority": "https://<your-domain>.kinde.com",
           "ValidAudiences": [
             "<your-audience>
           ]
         }
       }
     }
   ```

.NET authentication requires an audience. If you don’t already have an audience defined, you will need to do this by [registering your API](/developer-tools/your-apis/register-manage-apis/).

## Manage authorization with policies

Access tokens contain information (claims) about what a user is authorized to do when they sign in. You can create policies to manage authorization.

### Via permission claims (recommended)

Create a policy that allows only users with certain permission claims, e.g. `read:weather`:

```csharp
builder.Services
    .AddAuthorization(options =>
    {
      options.AddPolicy("ReadWeatherPermission",
        policy => policy.RequireAssertion(
          context => context.User.Claims.Any(c => c.Type == "permissions" && c.Value == "read:weather")
        ));
    });
```

[Set up permissions](/manage-users/roles-and-permissions/user-permissions/) in Kinde.

### Via role claims

1. [Set up Roles](/manage-users/roles-and-permissions/user-permissions/) in Kinde.
2. Add roles to the access token via custom claims, see the [token customization](/build/tokens/token-customization/) procedure.
3. Create a policy for a particular role, for example:

```csharp
builder.Services
    .AddAuthorization(options =>
    {
      options.AddPolicy("AdminRole",
        policy => policy.RequireAssertion(
          context => context.User.Claims.Any(c => c.Type == "roles" && c.Value == "admin")
        ));
    });
```

Note roles defined in Kinde do not map to roles as defined in ASP.NET, so the related functionality, such as `RequireRole()`, cannot be used.

## Secure controller-based APIs

To protect routes, add the `[Authorize]` attribute (from the `Microsoft.AspNetCore.Authorization` package) to any controllers or actions required.

For example, allow access only to users that satisfy the policy defined in the previous section:

```csharp
[Authorize(Policy = "ReadWeatherPermission")]
[HttpGet()]
public IEnumerable<WeatherForecast> Get()
```

See the [ASP.NET](http://ASP.NET) Core documentation for more details on [authorization](https://learn.microsoft.com/en-us/aspnet/core/security/authorization/simple?view=aspnetcore-8.0#use-the-authorize-attribute).

## Secure minimal APIs

To protect routes for minimal APIs, the `RequireAuthorization()` method can be called, and optionally pass in a policy, for example:

```csharp
app.MapGet("/protected", () => "Hello")
  .RequireAuthorization();
app.MapGet("/requires_specific_permission", () => "G'day")
  .RequireAuthorization("ReadWeatherPermission");
```

See the ASP.NET Core documentation for more details about [securing minimal APIs](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/security?view=aspnetcore-8.0).

## Swagger UI

Swagger UI provides an easy way to test APIs while in development.

For secured APIs, an access token is sent from your front end application with each request. Swagger UI can be configured to initiate auth and populate the token for you.

1. Configure Swagger with the details of Kinde’s OpenID Connect configuration:

   ```csharp
   builder.Services.AddSwaggerGen(options =>
   {
     var authority = builder.Configuration["Authentication:Schemes:Bearer:Authority"];
     options.AddSecurityDefinition(
       "oidc",
       new OpenApiSecurityScheme
       {
         Type = SecuritySchemeType.OpenIdConnect,
         OpenIdConnectUrl = new Uri(authority + "/.well-known/openid-configuration")
       }
     );

     options.AddSecurityRequirement(
       new OpenApiSecurityRequirement
       {
         {
           new OpenApiSecurityScheme
           {
             Reference = new OpenApiReference
               { Type = ReferenceType.SecurityScheme, Id = "oidc" },
           },
           new string[] { }
         }
       }
     );
   });
   ```

2. Configure Swagger UI to pre-populate the relevant parameters:

   ```csharp
   app.UseSwaggerUI(c =>
   {
     c.OAuthClientId("<frontend-app-client-id>");
     c.OAuthAdditionalQueryStringParams(new Dictionary<string, string>
       { { "audience", builder.Configuration["Authentication:Schemes:Bearer:ValidAudiences:0"] ?? "" } });
     c.OAuthUsePkce();
   });
   ```

3. Swap `<frontend-app-client-id>` with your front end application’s client ID shown in Kinde.
4. Add the callback URL to your front end application’s [allowed callbacks](/get-started/connect/callback-urls/) so that the Swagger UI initiates auth and returns to Swagger UI after auth.

   For local development, the callback will look like the following, though the port may differ:

   ```csharp
   https://localhost:7179/swagger/oauth2-redirect.html
   ```

5. Once this is set up, use the `Authorize` button to initiate auth. On return, API requests should be automatically populated with the access token. Note for front end applications, implicit flow is not supported, only PKCE is supported.

## Troubleshooting

When mismatched versions of dependent libraries are resolved, requests to the API may fail with HTTP 401 and a token validation error. It will look something like this:

```csharp
Bearer error="invalid_token",error_description="The signature key was not found
```

For example, if version 7.4.0 or later of `System.IdentityModel.Tokens.Jwt` is installed with a version of `Microsoft.AspNetCore.Authentication.JwtBearer` that is dependent on an earlier version of `Microsoft.IdentityModel.Protocols.OpenIdConnect`.

To fix, you would update the version of `Microsoft.IdentityModel.Protocols.OpenIdConnect` to match your version of `System.IdentityModel.Tokens.Jwt`. E.g. ensure they are both 7.4.0.
