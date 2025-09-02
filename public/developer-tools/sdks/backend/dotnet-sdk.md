
{/* @case-police-ignore Api */}
{/* @case-police-ignore Mvc */}
{/* @case-police-ignore Html */}

Kinde auth can be integrated into your application without the need for an SDK. Follow the appropriate guide to secure your API or use OpenID Connect to secure your web application:

- [Integrate with ASP.NET based APIs](/developer-tools/your-apis/dotnet-based-apis/)
- [Integrate with ASP.NET using Open ID Connect](/developer-tools/guides/dotnet-open-id-connect/)

The Kinde .NET SDK allows developers to quickly and securely call the Kinde Management API. The Kinde SDK is available from the Nuget package repository at [https://www.nuget.org/packages/Kinde.SDK](https://www.nuget.org/packages/Kinde.SDK)

## Before you begin

- Kinde .NET SDK supports .NET 6.0+
- If you haven’t already got a Kinde account, [register for free here](https://app.kinde.com/register) (no credit card required). Registering gives you a Kinde domain, which you need to get started, e.g. `yourapp.kinde.com`.
- Create a [machine to machine application for Kinde Management API access](/developer-tools/kinde-api/connect-to-kinde-api/).

## Add packages to your application

Use `Dotnet CLI` or `NuGet CLI` to add packages in your project.

Dotnet CLI:

```bash
dotnet add package Kinde.SDK
```

NuGet:

```bash
NuGet\Install-Package Kinde.SDK
```

This command is intended to be used within the Package Manager Console in Visual Studio, as it uses the NuGet module's version of Install-Package.

## Configure API client

Create and authorize a new client:

```csharp
var client = new KindeClient(new ApplicationConfiguration("<your-kinde-domain>", "", ""), new KindeHttpClient());
await client.Authorize(new ClientCredentialsConfiguration("<your-client-id>", "", "<your-client-secret>", "https://<your-kinde-domain>/api"));
```

Replacing `<your-kinde-domain>` with your Kinde domain, `<your-client-id>` and `<your-client-secret>` with your machine to machine app keys.

## Call management API

The client can be used to call the management API. For example, listing users:

```csharp
var usersApi = new UsersApi(client);
var users = await usersApi.GetUsersAsync();
foreach (var user in users.Users)
{
    Console.WriteLine($"Id: {user.Id}, Email: {user.Email}, Name: {user.FirstName} {user.LastName}.");
}
```

Note this requires the `read:users` scope to be added to the API in the machine to machine application in Kinde.

An example of creating an organization then renaming it:

```csharp
var orgApi = new OrganizationsApi(client);
var newOrg = await orgApi.CreateOrganizationAsync(new CreateOrganizationRequest("My new organization"));

var response = await orgApi.UpdateOrganizationAsync(newOrg.Organization.Code, new UpdateOrganizationRequest()
{
    Name = "A different name
});
```

Note this requires the `create:organizations` and `update:organizations` scopes to be added to the API in the machine to machine application in Kinde.

For full details of the available management API functions, see the [Kinde Management API specification](/kinde-apis/management/).

If you need help getting Kinde connected, contact us at [support@kinde.com](mailto:support@kinde.com).
