# Trusona OpenID Connect Example using .NET Core

This project demonstrates integrating ASP.NET and Trusona using the built-in
OpenID Connect components.

Signup for a Trusona Developers account at https://developers.trusona.com/

## Requirements

* .NET Core 2.1
* Trusona Developers Account
* Trusona ClientID

## Running the Example

1. Checkout the project from Git
1. Edit `appsettings.json` and add your `ClientID` value
1. Execute `dotnet run --project trusona-gateway-example`
1. Open a browser and navigate to `https://localhost:5001`

## Technical Information

The following snippet descibes how you configure .NET to use OpenID Connect for
integration with Trusona.

On startup, the framework will auto-configure using Trusona's metadata address and
the your Client ID.

In this example we are requesting an e-mail address and profile information in addition
to the unique `nameidentifier`.

```csharp
services
  .AddAuthentication(options =>
  {
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
  })
  .AddCookie()
  .AddOpenIdConnect(options =>
  {
    options.ClientId = "<client_id>";
    options.MetadataAddress = "https://gateway.trusona.net/oidc/.well-known/openid-configuration";
    options.Scope.Add("email");
    options.Scope.Add("profile");
    options.ResponseMode = OpenIdConnectResponseMode.FormPost;
    options.ResponseType = OpenIdConnectResponseType.IdToken;
    options.TokenValidationParameters = new TokenValidationParameters()
    {
      ValidateIssuer = true
    };
  });
```