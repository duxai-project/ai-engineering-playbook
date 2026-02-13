# 03 - Auth and Security

## Purpose of this chapter

Map authentication and authorization concepts to concrete ASP.NET Core implementation points in this solution.

## Authentication

### In Clean Architecture

Authentication answers: who is the user?

### In .NET

ASP.NET Core authentication middleware and Identity endpoints establish user identity and issue/validate credentials.

### In this project

Presentation setup:

- `builder.Services.AddAuthentication()`
- `app.MapGroup("api/identity").MapIdentityApi<User>()`

This exposes Identity API endpoints under `api/identity`.

Identity backing store configuration in `src/Restaurants.Infrastructure/Extensions/ServiceCollectionExtensions.cs`:

- `AddIdentityApiEndpoints<User>()`
- `AddRoles<IdentityRole>()`
- `AddClaimsPrincipalFactory<RestaurantsUserClaimsPrincipalFactory>()`
- `AddEntityFrameworkStores<RestaurantsDbContext>()`

## Authorization

### In Clean Architecture

Authorization answers: what is this authenticated user allowed to do?

### In .NET

Authorization can be attribute-based (`[Authorize]`) with roles and policies, plus resource-level handlers for fine-grained checks.

### In this project

Applied with:

- `[Authorize]` on controllers/actions
- role checks (example: `[Authorize(Roles = UserRoles.Owner)]`)
- policy checks (example: `PolicyNames.AtLeast20`)

## Policies and requirements

### In Clean Architecture

Access rules should be explicit and centralized, not spread as ad-hoc conditionals.

### In .NET

Policies are registered in DI and enforced through handlers/requirements.

### In this project

Configured policies:

- `HasNationality`: requires `Nationality` claim in allowed values
- `AtLeast20`: minimum age requirement
- `CreatedAtleast2Restaurants`: minimum created-restaurants requirement

Policy constants:

- `src/Restaurants.Infrastructure/Authorization/Constants.cs`

Custom handlers/services:

- `MinimumAgeRequirementHandler`
- `CreatedMultipleRestaurantsRequirementHandler`
- `RestaurantAuthorizationService` (resource-level authorization)

Related files:

- `src/Restaurants.Infrastructure/Authorization/Requirements/MinimumAgeRequirement.cs`
- `src/Restaurants.Infrastructure/Authorization/Requirements/MinimumAgeRequirementHandler.cs`
- `src/Restaurants.Infrastructure/Authorization/Requirements/CreatedMultipleRestaurantsRequirement.cs`
- `src/Restaurants.Infrastructure/Authorization/Requirements/CreatedMultipleRestaurantsRequirementHandler.cs`
- `src/Restaurants.Infrastructure/Authorization/Services/RestaurantAuthorizationService.cs`

## Policy status note in this codebase

`PolicyNames.CreatedAtleast2Restaurants`:

- defined in `src/Restaurants.Infrastructure/Authorization/Constants.cs`
- registered in `src/Restaurants.Infrastructure/Extensions/ServiceCollectionExtensions.cs`
- controller usage currently commented in `src/Restaurants.API/Controllers/RestaurantsController.cs`

To activate it on the endpoint:

- uncomment `[Authorize(Policy = PolicyNames.CreatedAtleast2Restaurants)]`

## Security notes for course documentation

- Keep token/auth setup documented in one place (`appsettings` + startup).
- Separate identity (authentication) from permissions (authorization).
- Document endpoint examples by access level: anonymous, authenticated, role-protected, policy-protected.

## Practical trade-offs

- Role-based checks are fast to apply but can grow messy at scale.
- Policy-based authorization is more expressive and testable.
- Resource-level authorization adds clarity for ownership rules, with extra implementation cost.
