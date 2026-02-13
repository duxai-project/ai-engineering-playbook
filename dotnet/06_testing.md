# 06 - Testing

## Purpose of this chapter

Define a practical test strategy by layer and map existing tests to architecture boundaries.

## Test architecture

### In Clean Architecture

Tests should validate behavior at the right boundary:

- Domain: business rules
- Application: use cases and validation
- API: HTTP contracts and middleware behavior
- Infrastructure: adapters and integrations

### In .NET

Use separate test projects per layer to keep scope clear and failures actionable.

### In this project

Test projects:

- `tests/Restaurants.API.Tests`
- `tests/Restaurants.Application.Tests`
- `tests/Restaurants.Infrastructure.Tests`

## Current coverage

### In this project

- API middleware behavior (`ErrorHandlingMiddlewareTests`)
- API controller behavior (`RestaurantsControllerTests`)
- Application handlers and validators
- AutoMapper profiles
- User context and current user services
- Infrastructure authorization requirement handlers

## Test file map

API:

- `tests/Restaurants.API.Tests/Middlewares/ErrorHandlingMiddlewareTests.cs`
- `tests/Restaurants.API.Tests/Controllers/RestaurantsControllerTests.cs`

Application:

- `tests/Restaurants.Application.Tests/Restaurants/Commands/CreateRestaurant/CreateRestaurantCommandHandlerTests.cs`
- `tests/Restaurants.Application.Tests/Restaurants/Commands/CreateRestaurant/CreateRestaurantCommandValidatorTests.cs`
- `tests/Restaurants.Application.Tests/Restaurants/Commands/UpdateRestaurant/UpdateRestaurantCommandHandlerTests.cs`
- `tests/Restaurants.Application.Tests/Restaurants/Dtos/RestaurantsProfileTests.cs`
- `tests/Restaurants.Application.Tests/Users/CurrentUserTests.cs`
- `tests/Restaurants.Application.Tests/Users/UserContextTests.cs`

Infrastructure:

- `tests/Restaurants.Infrastructure.Tests/Authorization/Requirements/CreatedMultipleRestaurantsRequirementHandlerTests.cs`

## Suggested strategy for new features

1. Add at least one Application test (handler or validator).
2. Add one API behavior test for endpoint contract.
3. Add Infrastructure test only when adapter logic is non-trivial.
4. Add Domain tests when pure domain services/rules are introduced.

## Example commands

```bash
dotnet test
dotnet test tests/Restaurants.Application.Tests/Restaurants.Application.Tests.csproj
dotnet test tests/Restaurants.API.Tests/Restaurants.API.Tests.csproj
```

## Documentation habit

For each new feature, update:

- one command/query test
- one API behavior test
- endpoint note in `05_api_reference.md`
- artifact entry in `08_artifacts_catalog.md`

## Practical trade-offs

- Too many end-to-end tests slow feedback loops; keep most tests in Application/API units.
- Mocking everything can hide integration bugs; add targeted integration tests for risky paths.
- Test names should describe behavior, not implementation details, to reduce rewrite churn.
