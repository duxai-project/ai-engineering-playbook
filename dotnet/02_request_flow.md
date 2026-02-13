# 02 - Request Flow

## Purpose of this chapter

Explain the full request lifecycle and show how each layer participates without breaking Clean Architecture boundaries.

## End-to-end flow (example: create restaurant)

### In Clean Architecture

A request should move from delivery mechanism to use case, then to infrastructure adapters, and back.

### In .NET

ASP.NET Core controller receives HTTP input, MediatR dispatches command/query, validators enforce input rules, and handlers coordinate repositories/services.

### In this project

1. Client sends `POST /api/restaurants`.
2. `RestaurantsController` receives the HTTP request.
3. Controller sends `CreateRestaurantCommand` through `IMediator`.
4. `CreateRestaurantCommandValidator` validates input.
5. `CreateRestaurantCommandHandler` executes the use case.
6. Handler uses `IRestaurantsRepository` (domain contract).
7. Infrastructure repository persists via EF Core `RestaurantsDbContext`.
8. Handler returns new `id`.
9. Controller returns `201 Created`.

## Responsibilities by layer

### In Clean Architecture

Each layer owns one concern and avoids leaking responsibilities into other layers.

### In .NET

- API: transport concerns (`HTTP`, routing, status codes, auth attributes)
- Application: use case orchestration, validation, DTO mapping
- Infrastructure: persistence and external adapters

### In this project

- API: controllers and middleware in `Restaurants.API`
- Application: commands/queries/validators/handlers in `Restaurants.Application`
- Infrastructure: repositories and EF Core in `Restaurants.Infrastructure`

## Middleware pipeline

### In Clean Architecture

Cross-cutting concerns (error handling, logging, security) should be centralized and reusable.

### In .NET

Middleware order defines behavior and can change security and response outcomes.

### In this project

From `Program.cs`:

1. `ErrorHandlingMiddleware`
2. `RequestTimeLoggingMiddleware`
3. `UseSerilogRequestLogging`
4. Swagger (development only)
5. `UseHttpsRedirection`
6. Identity API group mapping (`api/identity`)
7. `UseAuthorization`
8. `MapControllers`

## Why this flow matters

- Validation is isolated in validators.
- Business logic is isolated in handlers.
- Data access is behind repository contracts.
- HTTP concerns stay in controllers/middleware.

This separation improves testability, maintainability, and onboarding speed.

## Practical trade-offs

- For small APIs, full CQRS for every endpoint can be overkill.
- If mapping logic is tiny, avoid over-engineering with too many DTO variants.
- Keep middleware focused; too much logic in middleware can hide business behavior.
