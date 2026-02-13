# 08 - Artifacts Catalog (.NET + Clean Architecture)

This chapter maps the concrete artifacts used in this solution and where they live.

## 1) Solution and project artifacts

- `Restaurants.sln`: solution root.
- `src/Restaurants.API/Restaurants.API.csproj`: presentation/web host.
- `src/Restaurants.Application/Restaurants.Application.csproj`: use cases and orchestration.
- `src/Restaurants.Domain/Restaurants.Domain.csproj`: domain model and contracts.
- `src/Restaurants.Infrastructure/Restaurants.Infrastructure.csproj`: technical implementations.

## 2) API/Presentation artifacts

Core files:

- `src/Restaurants.API/Program.cs`: app bootstrap, middleware pipeline, endpoint mapping.
- `src/Restaurants.API/Extensions/WebApplicationBuilderExtensions.cs`: presentation service registration, Swagger, Serilog.

Controllers:

- `src/Restaurants.API/Controllers/RestaurantsController.cs`
- `src/Restaurants.API/Controllers/DishesController.cs`
- `src/Restaurants.API/Controllers/IdentityController.cs`

Middlewares:

- `src/Restaurants.API/Middlewares/ErrorHandlingMiddleware.cs`
- `src/Restaurants.API/Middlewares/RequestTimeLoggingMiddleware.cs`

Other API assets:

- `src/Restaurants.API/Restaurants.API.http`: HTTP request playground.
- `assets/swagger.json`: generated OpenAPI snapshot.

## 3) Application artifacts (CQRS + validation + mapping)

Application service registration:

- `src/Restaurants.Application/Extensions/ServiceCollectionExtension.cs`

Common models:

- `src/Restaurants.Application/Common/PagedResult.cs`

### Restaurants feature

Commands and handlers:

- `src/Restaurants.Application/Restaurants/Commands/CreateRestaurant/CreateRestaurantCommand.cs`
- `src/Restaurants.Application/Restaurants/Commands/CreateRestaurant/CreateRestaurantCommandHandler.cs`
- `src/Restaurants.Application/Restaurants/Commands/UpdateRestaurant/UpdateRestaurantCommand.cs`
- `src/Restaurants.Application/Restaurants/Commands/UpdateRestaurant/UpdateRestaurantCommandHandler.cs`
- `src/Restaurants.Application/Restaurants/Commands/DeleteRestaurant/DeleteRestaurantCommand.cs`
- `src/Restaurants.Application/Restaurants/Commands/DeleteRestaurant/DeleteRestaurantCommandHandler.cs`
- `src/Restaurants.Application/Restaurants/Commands/UploadRestaurantLogo/UploadRestaurantLogoCommand.cs`
- `src/Restaurants.Application/Restaurants/Commands/UploadRestaurantLogo/UploadRestaurantLogoCommandHandler.cs`

Queries and handlers:

- `src/Restaurants.Application/Restaurants/Queries/GetAllRestaurants/GetAllRestaurantsQuery.cs`
- `src/Restaurants.Application/Restaurants/Queries/GetAllRestaurants/GetAllRestaurantsQueryHandler.cs`
- `src/Restaurants.Application/Restaurants/Queries/GetRestaurantById/GetRestaurantByIdQuery.cs`
- `src/Restaurants.Application/Restaurants/Queries/GetRestaurantById/GetRestaurantByIdQueryHandler.cs`

Validators:

- `src/Restaurants.Application/Restaurants/Commands/CreateRestaurant/CreateRestaurantCommandValidator.cs`
- `src/Restaurants.Application/Restaurants/Commands/UpdateRestaurant/UpdateRestaurantCommandValidator.cs`
- `src/Restaurants.Application/Restaurants/Queries/GetAllRestaurants/GetAllRestaurantsQueryValidator.cs`

DTO and mapping:

- `src/Restaurants.Application/Restaurants/Dtos/RestaurantDto.cs`
- `src/Restaurants.Application/Restaurants/Dtos/RestaurantsProfile.cs`

### Dishes feature

Commands and handlers:

- `src/Restaurants.Application/Dishes/Commands/CreateDish/CreateDishCommand.cs`
- `src/Restaurants.Application/Dishes/Commands/CreateDish/CreateDishCommandHandler.cs`
- `src/Restaurants.Application/Dishes/Commands/DeleteDishes/DeleteDishesForRestaurantCommand.cs`
- `src/Restaurants.Application/Dishes/Commands/DeleteDishes/DeleteDishesForRestaurantCommandHandler.cs`

Queries and handlers:

- `src/Restaurants.Application/Dishes/Queries/GetDishesForRestaurant/GetDishesForRestaurantQuery.cs`
- `src/Restaurants.Application/Dishes/Queries/GetDishesForRestaurant/GetDishesForRestaurantQueryHandler.cs`
- `src/Restaurants.Application/Dishes/Queries/GetDishByIdForRestaurant/GetDishByIdForRestaurantQuery.cs`
- `src/Restaurants.Application/Dishes/Queries/GetDishByIdForRestaurant/GetDishByIdForRestaurantQueryHandler.cs`

Validators:

- `src/Restaurants.Application/Dishes/Commands/CreateDish/CreateDishCommandValidator.cs`

DTO and mapping:

- `src/Restaurants.Application/Dishes/Dtos/DishDto.cs`
- `src/Restaurants.Application/Dishes/Dtos/DishesProfile.cs`

### Users feature

Context and identity helpers:

- `src/Restaurants.Application/Users/UserContext.cs`
- `src/Restaurants.Application/Users/CurrentUser.cs`

Commands and handlers:

- `src/Restaurants.Application/Users/Commands/UpdateUserDetails/UpdateUserDetailsCommand.cs`
- `src/Restaurants.Application/Users/Commands/UpdateUserDetails/UpdateUserDetailsCommandHandler.cs`
- `src/Restaurants.Application/Users/Commands/AssignUserRole/AssignUserRoleCommand.cs`
- `src/Restaurants.Application/Users/Commands/AssignUserRole/AssignUserRoleCommandHandler.cs`
- `src/Restaurants.Application/Users/Commands/UnassignUserRole/UnassignUserRoleCommand.cs`
- `src/Restaurants.Application/Users/Commands/UnassignUserRole/UnassignUserRoleCommandHandler.cs`

## 4) FluentValidation artifacts

Framework wiring:

- `src/Restaurants.Application/Extensions/ServiceCollectionExtension.cs`
  - `AddValidatorsFromAssembly(...)`
  - `AddFluentValidationAutoValidation()`

Validators currently in project:

- `CreateRestaurantCommandValidator`
- `UpdateRestaurantCommandValidator`
- `CreateDishCommandValidator`
- `GetAllRestaurantsQueryValidator`

Behavior summary:

- Validators run automatically for API-bound request models.
- Validation rules include string length, allowed categories, email format, postal code regex, paging constraints and non-negative numeric checks.

## 5) Domain artifacts

Entities:

- `src/Restaurants.Domain/Entities/Restaurant.cs`
- `src/Restaurants.Domain/Entities/Dish.cs`
- `src/Restaurants.Domain/Entities/Address.cs`
- `src/Restaurants.Domain/Entities/User.cs`

Domain contracts:

- `src/Restaurants.Domain/Repositories/IRestaurantsRepository.cs`
- `src/Restaurants.Domain/Repositories/IDishesRepository.cs`
- `src/Restaurants.Domain/Interfaces/IRestaurantAuthorizationService.cs`
- `src/Restaurants.Domain/Interfaces/IBlobStorageService.cs`

Constants and enums:

- `src/Restaurants.Domain/Constants/UserRoles.cs`
- `src/Restaurants.Domain/Constants/ResourceOperation.cs`
- `src/Restaurants.Domain/Constants/SortDirection.cs`

Exceptions:

- `src/Restaurants.Domain/Exceptions/NotFoundException.cs`
- `src/Restaurants.Domain/Exceptions/ForbidException.cs`

## 6) Infrastructure artifacts

Service registration:

- `src/Restaurants.Infrastructure/Extensions/ServiceCollectionExtensions.cs`

Persistence:

- `src/Restaurants.Infrastructure/Persistence/RestaurantsDbContext.cs`
- `src/Restaurants.Infrastructure/Migrations/*`

Repositories:

- `src/Restaurants.Infrastructure/Repositories/RestaurantsRepository.cs`
- `src/Restaurants.Infrastructure/Repositories/DishesRepository.cs`

Authorization:

- `src/Restaurants.Infrastructure/Authorization/Constants.cs`
- `src/Restaurants.Infrastructure/Authorization/RestaurantsUserClaimsPrincipalFactory.cs`
- `src/Restaurants.Infrastructure/Authorization/Requirements/MinimumAgeRequirement.cs`
- `src/Restaurants.Infrastructure/Authorization/Requirements/MinimumAgeRequirementHandler.cs`
- `src/Restaurants.Infrastructure/Authorization/Requirements/CreatedMultipleRestaurantsRequirement.cs`
- `src/Restaurants.Infrastructure/Authorization/Requirements/CreatedMultipleRestaurantsRequirementHandler.cs`
- `src/Restaurants.Infrastructure/Authorization/Services/RestaurantAuthorizationService.cs`

Seeders:

- `src/Restaurants.Infrastructure/Seeders/IRestaurantSeeder.cs`
- `src/Restaurants.Infrastructure/Seeders/RestaurantSeeder.cs`

Blob storage:

- `src/Restaurants.Infrastructure/Configuration/BlobStorageSettings.cs`
- `src/Restaurants.Infrastructure/Storage/BlobStorageService.cs`

## 7) Policy artifacts and `CreatedAtleast2Restaurants`

Definition:

- `src/Restaurants.Infrastructure/Authorization/Constants.cs` contains:
  - `PolicyNames.CreatedAtleast2Restaurants`

Registration:

- `src/Restaurants.Infrastructure/Extensions/ServiceCollectionExtensions.cs` contains:
  - `.AddPolicy(PolicyNames.CreatedAtleast2Restaurants, builder => builder.AddRequirements(new CreatedMultipleRestaurantsRequirement(2)))`

Requirement + handler:

- `src/Restaurants.Infrastructure/Authorization/Requirements/CreatedMultipleRestaurantsRequirement.cs`
- `src/Restaurants.Infrastructure/Authorization/Requirements/CreatedMultipleRestaurantsRequirementHandler.cs`

Current controller usage:

- In `src/Restaurants.API/Controllers/RestaurantsController.cs`, the attribute line for this policy exists but is commented.

## 8) Test artifacts

Test projects:

- `tests/Restaurants.API.Tests/Restaurants.API.Tests.csproj`
- `tests/Restaurants.Application.Tests/Restaurants.Application.Tests.csproj`
- `tests/Restaurants.Infrastructure.Tests/Restaurants.Infrastructure.Tests.csproj`

Representative tests:

- API middleware/controller tests
- application command/query/validator tests
- mapping tests
- authorization requirement handler tests

## 9) Supporting assets

- `assets/INSERT Restaurants.sql`: SQL helper script.
- `.devcontainer/*`: development container configuration.
- `.github/*`: CI/CD and repository automation config.
- `LICENSE`: MIT license.
