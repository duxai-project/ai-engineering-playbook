# 07 - Exercises

Use these exercises to practice the same architecture style.

## Exercise 1: Add "Deactivate Restaurant"

Goal:

- Add an `IsActive` field to `Restaurant`
- Create a command to deactivate a restaurant
- Protect endpoint so only owner/admin can deactivate

Expected files touched:

- Domain entity + migration
- Application command/handler/validator
- API endpoint
- tests

## Exercise 2: Add "Get My Restaurants"

Goal:

- Return restaurants created by current user

Hints:

- Use `IUserContext` in application layer
- Create query + handler
- Keep controller thin

## Exercise 3: Add restaurant logo delete endpoint

Goal:

- `DELETE /api/restaurants/{id}/logo`
- Remove file from blob storage
- Handle not-found and forbidden cases

## Exercise 4: Add pagination response metadata

Goal:

- Include `totalItems`, `totalPages`, `pageNumber`, `pageSize`
- Keep backward-compatible response contract if possible

## Exercise 5: Add observability notes

Goal:

- Document log levels and correlation id strategy
- Add examples for request timing log output

