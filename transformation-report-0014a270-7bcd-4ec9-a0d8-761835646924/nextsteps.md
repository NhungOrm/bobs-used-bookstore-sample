# Next Steps

## Issues resolved
- Transformed Bookstore.Domain.csproj to net8.0
- Transformed Bookstore.Data.csproj to net8.0
- Transformed Bookstore.Web.csproj to net8.0
- Transformed Bookstore.Cdk.csproj to net8.0
- Transformed Bookstore.Domain.Tests.csproj to net8.0

## Overview

The solution appears to have transformed successfully. No build errors were detected across any of the five projects:

- `Bookstore.Data`
- `Bookstore.Domain.Tests`
- `Bookstore.Cdk`
- `Bookstore.Web`
- `Bookstore.Domain`

The following steps outline how to validate, test, and deploy the migrated solution.

---

## 1. Restore Dependencies

Run a NuGet package restore to ensure all dependencies are resolved correctly before proceeding with any builds or tests.

```bash
dotnet restore
```

Review the output for any warnings related to package compatibility or deprecated packages targeting older frameworks.

---

## 2. Build the Entire Solution

Perform a full solution build to confirm there are no compilation issues that may not have surfaced during the initial transformation analysis.

```bash
dotnet build --configuration Release
```

Address any warnings that appear, particularly those related to nullable reference types, deprecated APIs, or platform compatibility.

---

## 3. Run the Test Suite

Execute the tests in `Bookstore.Domain.Tests` to verify that domain logic behaves correctly after migration.

```bash
dotnet test app/Bookstore.Domain.Tests/Bookstore.Domain.Tests.csproj --configuration Release --verbosity normal
```

- Review any failing tests and determine whether failures are due to migration changes or pre-existing issues.
- Check that test dependencies and any mocking frameworks (e.g., Moq, NSubstitute) are referencing compatible .NET versions.

---

## 4. Validate the Data Layer

Open `Bookstore.Data` and verify the following:

- Any Entity Framework Core migrations are up to date. Run the following to check pending migrations:

```bash
dotnet ef migrations list --project app/Bookstore.Data/Bookstore.Data.csproj
```

- If the project previously used Entity Framework 6 (EF6), confirm it has been migrated to Entity Framework Core and that the `DbContext`, models, and queries function as expected.
- Run a test against a local or development database to confirm data access operations work correctly.

---

## 5. Validate the Web Project

Run the `Bookstore.Web` project locally to verify that the application starts and core functionality is accessible.

```bash
dotnet run --project app/Bookstore.Web/Bookstore.Web.csproj --configuration Release
```

- Navigate through the key pages and features of the application.
- Check for any runtime errors related to middleware configuration, dependency injection registrations, or static file handling.
- If the project previously used ASP.NET Web Forms or ASP.NET MVC 5, confirm that the migration to ASP.NET Core MVC or Razor Pages is complete and that routing, model binding, and authentication behave as expected.

---

## 6. Review the CDK Project

Inspect `Bookstore.Cdk` to ensure infrastructure definitions are accurate for the target deployment environment.

- Confirm that any environment-specific configuration values (e.g., connection strings, region settings) are correctly defined.
- Validate that the CDK stack synthesizes without errors:

```bash
dotnet build app/Bookstore.Cdk/Bookstore.Cdk.csproj --configuration Release
```

---

## 7. Check Configuration Files

Review the following configuration concerns across the solution:

- Confirm `appsettings.json` and `appsettings.Production.json` contain the correct values and that no settings were lost from legacy `Web.config` or `App.config` files.
- Verify that connection strings, API keys, and environment-specific settings are properly managed, preferably using environment variables or a secrets manager rather than hardcoded values.

---

## 8. Deploy to a Staging Environment

Once local validation is complete, deploy the application to a staging environment that mirrors production.

- Use the CDK project to provision infrastructure if applicable:

```bash
cdk deploy
```

- Run smoke tests against the staging environment to confirm end-to-end functionality before promoting to production.