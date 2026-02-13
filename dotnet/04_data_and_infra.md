# 04 - Data and Infrastructure

## Purpose of this chapter

Explain how persistence and external services are implemented while preserving Clean Architecture boundaries.

## Persistence

### In Clean Architecture

Infrastructure owns database details. Application and Domain consume abstractions, not ORM APIs.

### In .NET

EF Core `DbContext`, migrations, and provider-specific config stay in Infrastructure.

### In this project

- `RestaurantsDbContext`
- migrations in `src/Restaurants.Infrastructure/Migrations`
- connection string key: `ConnectionStrings:RestaurantsDb`

## Repository boundary

### In Clean Architecture

Use repository contracts as ports from use cases to data access.

### In .NET

Define interfaces in Domain/Application boundary and implement with EF Core in Infrastructure.

### In this project

Contracts:

- `IRestaurantsRepository`
- `IDishesRepository`

Implementations:

- `RestaurantsRepository`
- `DishesRepository`

This keeps application use cases independent from EF Core specifics.

## Seeding strategy

### In Clean Architecture

Bootstrap data loading is an infrastructure concern, not a use case concern.

### In .NET

Run a seeder at startup via scoped services.

### In this project

At startup, app creates a scope and runs:

- `IRestaurantSeeder.Seed()`

## External storage adapter

### In Clean Architecture

File/blob providers are external systems behind an interface.

### In .NET

Expose an abstraction and inject provider implementation through DI.

### In this project

- `IBlobStorageService` (abstraction)
- `BlobStorageService` (implementation)
- configuration section: `BlobStorage` in `appsettings*.json`

## Identity and persistence integration

### In Clean Architecture

Identity persistence is infrastructure and should not leak into use case logic.

### In .NET

Identity can use EF Core stores with DI registration.

### In this project

- `AddIdentityApiEndpoints<User>()`
- `AddRoles<IdentityRole>()`
- `AddEntityFrameworkStores<RestaurantsDbContext>()`

## Practical trade-offs

- Repositories improve isolation, but over-abstraction can duplicate EF Core features.
- Direct EF Core in handlers is faster initially, but weakens test boundaries over time.
- Seeding in startup is convenient for courses/dev; production often needs controlled migration/seed pipelines.
