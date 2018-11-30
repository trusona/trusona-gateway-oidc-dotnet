# Trusona OpenID Connect Example using .NET Core

This project demonstrates integrating ASP.NET and Trusona using the built-in
OpenID Connect components.

Signup for a Trusona Developers account at https://developers.trusona.com/

## Requirements

* .NET Core 2.1
* Trusona Developers Account

## Project setup

1. Checkout the project from GitHub
1. Email `support@trusona.com` and let them know that you want to run this example. Include the email that is displayed when you log in to your Trusona Developers account.
1. Edit `appsettings.json` and add your `ClientID` value
1. Execute `dotnet run --project trusona-gateway-example`

## Using the example

1. Open a browser and navigate to `https://localhost:5001`
1. Click the “Login with Trusona button” that you see on the screen
1. Open the Trusona App on your phone
1. Now that you’ve been redirect to the Trusona Gateway, use the Trusona App to scan the QR code on your screen
1. Tap "Accept" in the Trusona App
1. Now you’ve been redirected back to the example application running on your machine
1. See the user information displayed on the screen that indicates a successful login

## Technical information

The following snippet from `Startup.cs` descibes how you configure .NET to use OpenID Connect for integration with Trusona.

On startup, the framework will auto-configure using Trusona's metadata address and the your Client ID.

In this example we are requesting an e-mail address and profile information in addition to the unique `nameidentifier`.

Note: There is an outstanding issue with iOS and Safari and the .NET framework. This example as configured does not work on these devices. For more information, see https://github.com/aspnet/Security/issues/1864

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