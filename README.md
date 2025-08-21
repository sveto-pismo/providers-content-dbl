# SvetoPismo Providers — Digital Bible Library (DBL) Content

Welcome — we’re so glad you’re here! This repository hosts a friendly, modern .NET library that makes it straightforward to work with the Digital Bible Library (DBL) content APIs from your applications and services.

Whether you’re integrating Scripture content into a website, building a background service, or experimenting locally, we hope this library helps you move fast with clarity and confidence.

> Note: You must have an active DBL account and valid credentials/keys issued by the Digital Bible Library to access their APIs. Please review DBL’s documentation and terms of use before getting started.

## What is this repository?
This solution contains a reusable .NET 9.0 package:

- SvetoPismo.Providers.Content.DigitalBibleLibrary — building blocks for calling DBL content endpoints, designed to integrate cleanly with HttpClient and dependency injection.

For deeper, API-specific notes and a minimal usage sample, see the package README:
- Package README: `src/DigitalBibleLibrary/README.md`

## Features
- Clean HttpClient + dependency injection integration
- Practical guidance for authentication and configuration
- .NET 9.0 target with Source Link and symbols to improve debugging experience
- Open-source under Apache-2.0

## Quick start
If you just want to try the package, add it to your app and wire up an HttpClient. Replace base addresses, headers, and endpoints according to your DBL credentials and integration.

Install via .NET CLI:

```bash
dotnet add package SvetoPismo.Providers.Content.DigitalBibleLibrary
```

Minimal C# setup example:

```csharp
using System.Net.Http.Headers;
using Microsoft.Extensions.DependencyInjection;

var services = new ServiceCollection();

services.AddHttpClient("DBL", client =>
{
    client.BaseAddress = new Uri("https://api.digitalbiblelibrary.org/");
    // Configure authentication per DBL guidance for your account.
    // For example, if DBL uses API keys:
    // client.DefaultRequestHeaders.Add("X-Api-Key", Environment.GetEnvironmentVariable("DBL_API_KEY"));
    // Or if it uses bearer tokens:
    // client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
});

// Register typed clients/services here if the package exposes them.
// services.AddTransient<IDblContentClient, DblContentClient>();

var provider = services.BuildServiceProvider();
var http = provider.GetRequiredService<IHttpClientFactory>().CreateClient("DBL");

// Example call (replace with actual DBL endpoint and model):
var response = await http.GetAsync("v1/content");
response.EnsureSuccessStatusCode();
var json = await response.Content.ReadAsStringAsync();
```

More details and guidance are available in `src/DigitalBibleLibrary/README.md`.

## Configuration
- Base URL: Use the correct DBL API base URL for your environment (production or sandbox).
- Authentication: Configure the header(s) required by DBL (API key, bearer token, etc.). Avoid hardcoding secrets — prefer environment variables, user secrets, or a secrets manager.
- Resilience: Consider adding Polly policies (retries, circuit breaker) to the HttpClient for network resilience.

```csharp
services.AddHttpClient("DBL")
    .AddPolicyHandler(retryPolicy)
    .AddPolicyHandler(circuitBreakerPolicy);
```

## Build the solution locally
You’ll need the .NET SDK 9.0.

```bash
# Clone
git clone https://github.com/sveto-pismo/providers-content-dbl.git
cd providers-content-dbl

# Restore and build
dotnet restore
# You can also build the solution file explicitly
dotnet build Solution.slnx -c Release

# (Optional) Pack the NuGet package
# This emits a .nupkg you can use locally
 dotnet pack src/DigitalBibleLibrary/SvetoPismo.Providers.Content.DigitalBibleLibrary.csproj -c Release
```

## Repository structure
- `src/DigitalBibleLibrary/` — library source code and package-level README
- `LICENSE` — Apache-2.0 license
- `Directory.Build.props` / `Directory.Packages.props` — shared build and dependency configuration

## Contributing
We warmly welcome contributions — thank you for considering helping out!

- Start by opening an issue to discuss significant changes or new ideas.
- Use clear commit messages and keep PRs focused for easier reviews.
- Be kind and respectful. We value collaboration and a supportive, inclusive community.

By contributing, you agree that your contributions will be licensed under the Apache-2.0 license.

Issues: https://github.com/sveto-pismo/providers-content-dbl/issues

## License
Licensed under the Apache License, Version 2.0. See the `LICENSE` file for details.

## Trademarks
Digital Bible Library and related names may be trademarks of their respective owners. Use of this library does not imply endorsement by or affiliation with DBL.

## Thank you
Thanks for stopping by! We’re excited you’re here. If you have questions, ideas, or feedback, please open an issue — we’d love to hear from you.
