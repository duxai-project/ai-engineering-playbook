# 00 - Overview

## What this project is

`Restaurants API` is a .NET 8 Web API built using:

- Clean Architecture
- CQRS with MediatR
- Validation with FluentValidation
- EF Core + SQL Server
- ASP.NET Core Identity
- Authorization policies and custom handlers

## Main business capabilities

- Manage restaurants
- Manage dishes inside each restaurant
- Register/login users through Identity endpoints
- Assign/unassign roles
- Upload restaurant logo files

## Solution map

- `src/Restaurants.API`: controllers, middleware, startup wiring
- `src/Restaurants.Application`: commands, queries, handlers, validators, DTO mapping
- `src/Restaurants.Domain`: entities, contracts, constants, domain exceptions
- `src/Restaurants.Infrastructure`: EF Core, repositories, identity config, auth handlers, blob storage
- `tests/*`: layer-specific tests

## Why this is a good Clean Architecture example

- Dependencies point inward (API/Infrastructure depend on Application/Domain)
- Business use cases are explicit (`Command` and `Query` types)
- Domain contracts are implemented in Infrastructure
- Cross-cutting concerns are centralized (middlewares, logging, authorization)

