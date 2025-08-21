# SvetoPismo.Providers.Content.DigitalBibleLibrary

![CodeRabbit Pull Request Reviews](https://img.shields.io/coderabbit/prs/github/sveto-pismo/providers-content-dbl?utm_source=oss&utm_medium=github&utm_campaign=sveto-pismo%2Fproviders-content-dbl&labelColor=171717&color=FF570A&link=https%3A%2F%2Fcoderabbit.ai&label=CodeRabbit+Reviews&style=for-the-badge)
[![Sourcery](https://img.shields.io/badge/Sourcery-enabled-brightgreen?style=for-the-badge&labelColor=171717&color=ffa724)](https://sourcery.ai)


A .NET library intended to simplify working with the Digital Bible Library (DBL) content APIs from SvetoPismo providers.

This package provides the building blocks you can use to call DBL endpoints from your .NET applications and services. It is designed to integrate cleanly with HttpClient and dependency injection.

- Target framework: .NET 9.0
- License: Apache-2.0 (see LICENSE)
- Repository: https://github.com/sveto-pismo/providers-content-dbl

Note: You must have an active DBL account and valid credentials/keys issued by the Digital Bible Library to access their APIs. See DBL documentation and terms of use.

## Install

Using .NET CLI:

```
dotnet add package SvetoPismo.Providers.Content.DigitalBibleLibrary
```

Using PackageReference:

```
<ItemGroup>
  <PackageReference Include="SvetoPismo.Providers.Content.DigitalBibleLibrary" Version="$(LatestVersion)" />
</ItemGroup>
```

## Quick start

Below is a minimal example showing one way to wire up an HttpClient and a simple service that could call DBL endpoints. Replace base addresses, headers, and endpoints according to your DBL integration and credentials.

```csharp
using System.Net.Http.Headers;
using Microsoft.Extensions.DependencyInjection;

var services = new ServiceCollection();

services.AddHttpClient("DBL", client =>
{
    client.BaseAddress = new Uri("https://api.digitalbiblelibrary.org/");
    // Use the authentication mechanism provided by DBL for your account.
    // For example, if DBL uses API keys:
    // client.DefaultRequestHeaders.Add("X-Api-Key", Environment.GetEnvironmentVariable("DBL_API_KEY"));
    // Or if it uses bearer tokens:
    // client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
});

// If this package exposes typed clients or services, register them here.
// services.AddTransient<IDblContentClient, DblContentClient>();

var provider = services.BuildServiceProvider();
var http = provider.GetRequiredService<IHttpClientFactory>().CreateClient("DBL");

// Example call (replace with actual DBL endpoint and model):
var response = await http.GetAsync("v1/content");
response.EnsureSuccessStatusCode();
var json = await response.Content.ReadAsStringAsync();
```

## Configuration

- Base URL: Use the correct DBL API base URL for your environment (production or sandbox).
- Authentication: Configure the header(s) required by DBL (API key, bearer token, etc.). Do not hardcode secrets; prefer environment variables, user secrets, or a secrets manager.
- Resilience: Consider adding Polly policies (retries, circuit breaker) to the HttpClient for network resilience.

```csharp
services.AddHttpClient("DBL")
    .AddPolicyHandler(retryPolicy)
    .AddPolicyHandler(circuitBreakerPolicy);
```

## Versioning and compatibility

- Compiled for .NET 9.0.
- Symbols and Source Link are enabled to improve debugging experience.

## Build and source

- Repository: https://github.com/sveto-pismo/providers-content-dbl
- Issues: https://github.com/sveto-pismo/providers-content-dbl/issues

## Contributing

Contributions are welcome. Please open an issue to discuss significant changes first. By contributing, you agree that your contributions will be licensed under the Apache-2.0 license.

## License

Licensed under the Apache License, Version 2.0. See the LICENSE file for details.

## Trademarks

Digital Bible Library and related names may be trademarks of their respective owners. Use of this library does not imply endorsement by or affiliation with the DBL.
