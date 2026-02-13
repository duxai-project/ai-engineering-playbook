# 01 - Clean Architecture

## Purpose of this chapter

Define Clean Architecture rules and map each one to concrete .NET implementation points.

## Version note

These notes describe a .NET 8 style solution. If you read newer ASP.NET Core docs, adapt examples to your target version.

## Composition root

### In Clean Architecture

The outer layer wires dependencies. Use cases should not construct infrastructure dependencies directly.

### In .NET

Application startup and service registration are centralized in the composition root with dependency injection.

### In this project

`src/Restaurants.API/Program.cs`:

- `builder.AddPresentation()`
- `builder.Services.AddApplication()`
- `builder.Services.AddInfrastructure(builder.Configuration)`

## Layer 1: Domain

### In Clean Architecture

Domain contains enterprise rules, entities, value objects, and abstractions. It should not depend on frameworks.

### In .NET

Domain is typically a class library with entities, domain exceptions, constants, and interfaces.

### In this project

Examples:

- Entities: `Restaurant`, `Dish`, `User`, `Address`
- Interfaces: `IRestaurantsRepository`, `IDishesRepository`, `IRestaurantAuthorizationService`, `IBlobStorageService`
- Constants: roles, policies, resource operations
- Exceptions: `NotFoundException`, `ForbidException`

Rule: domain should not know EF Core, ASP.NET, controllers, or external services.

## Layer 2: Application

### In Clean Architecture

Application orchestrates use cases and depends on domain contracts.

### In .NET

Use cases are commonly implemented with CQRS, MediatR handlers, validators, and mapping profiles.

### In this project

Patterns used:

- CQRS (`Command`/`Query`)
- Handlers with MediatR
- Validation with FluentValidation
- Mapping to DTOs with AutoMapper

Registration in `src/Restaurants.Application/Extensions/ServiceCollectionExtension.cs`:

- `AddMediatR(...)`
- `AddAutoMapper(...)`
- `AddValidatorsFromAssembly(...).AddFluentValidationAutoValidation()`
- `AddScoped<IUserContext, UserContext>()`

Example handlers:

- `CreateRestaurantCommandHandler`
- `GetAllRestaurantsQueryHandler`
- `UpdateUserDetailsCommandHandler`

Rule: application depends on domain contracts, not infrastructure implementations.

## Layer 3: Infrastructure

### In Clean Architecture

Infrastructure implements ports/contracts and interacts with external systems.

### In .NET

This layer owns EF Core, cloud services, identity stores, repository implementations, and adapters.

### In this project

Examples:

- `RestaurantsDbContext` (EF Core)
- repository implementations
- authorization handlers and services
- blob storage service
- seeder

Registration in `src/Restaurants.Infrastructure/Extensions/ServiceCollectionExtensions.cs`:

- EF Core SQL Server `AddDbContext<RestaurantsDbContext>(...)`
- Identity endpoints storage `AddIdentityApiEndpoints<User>()`
- repositories and seeders
- authorization policies and handlers
- blob storage settings and service

Rule: infrastructure can depend on frameworks, but exposes abstractions consumed by upper layers.

## Layer 4: API (Presentation)

### In Clean Architecture

Presentation receives requests and delegates use cases without embedding business rules.

### In .NET

Controllers, middleware, endpoint mapping, and OpenAPI are implemented here.

### In this project

Examples:

- Controllers (`RestaurantsController`, `DishesController`, `IdentityController`)
- Middleware (`ErrorHandlingMiddleware`, `RequestTimeLoggingMiddleware`)
- Startup wiring in `Program.cs`
- OpenAPI/Swagger setup in `src/Restaurants.API/Extensions/WebApplicationBuilderExtensions.cs`

Rule: API should be thin and delegate business logic to application handlers.

## Dependency direction summary

`API -> Application -> Domain`  
`Infrastructure -> Application + Domain`  
`Domain` should not depend on outer layers.

## Package map by project

- `src/Restaurants.API/Restaurants.API.csproj`
  - `Serilog.AspNetCore`
  - `Serilog.Sinks.ApplicationInsights`
  - `Swashbuckle.AspNetCore`
- `src/Restaurants.Application/Restaurants.Application.csproj`
  - `MediatR`
  - `FluentValidation.AspNetCore`
  - `AutoMapper.Extensions.Microsoft.DependencyInjection`
- `src/Restaurants.Infrastructure/Restaurants.Infrastructure.csproj`
  - `Microsoft.EntityFrameworkCore.SqlServer`
  - `Azure.Storage.Blobs`
- `src/Restaurants.Domain/Restaurants.Domain.csproj`
  - `Microsoft.AspNetCore.Identity.EntityFrameworkCore`

## Pragmatic note on purity

If a framework package appears in Domain (for example Identity EF Core), document it as a conscious trade-off and explain impact:

- cleaner layering ideal: move framework dependency to Infrastructure
- pragmatic course decision: keep moving with less refactoring
- recommendation for production: isolate framework dependencies from Domain when possible

## Practical trade-offs

- Use CQRS when use cases are growing; avoid over-splitting for tiny CRUD APIs.
- Keep controllers thin; do not duplicate validation in multiple layers.
- Repositories are useful boundaries, but avoid abstracting every EF Core detail if it adds noise.
- Prefer explicit policies/requirements for authorization over scattered imperative checks.
