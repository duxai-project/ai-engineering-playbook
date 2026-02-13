# 05 - API Reference

## Restaurants

- `GET /api/restaurants`
  - Query: `searchPhrase`, `pageSize`, `pageNumber`, `sortBy`, `sortDirection`
  - Auth: anonymous allowed in current controller config
- `GET /api/restaurants/{id}`
  - Auth: requires authenticated user
- `POST /api/restaurants`
  - Auth: role `Owner`
- `PATCH /api/restaurants/{id}`
  - Auth: requires authenticated user
- `DELETE /api/restaurants/{id}`
  - Auth: requires authenticated user
- `POST /api/restaurants/{id}/logo`
  - Auth: requires authenticated user
  - Content: multipart/form-data (`file`)

## Dishes

Base route: `/api/restaurants/{restaurantId}/dishes`

- `POST /api/restaurants/{restaurantId}/dishes`
- `GET /api/restaurants/{restaurantId}/dishes`
  - Policy: `AtLeast20`
- `GET /api/restaurants/{restaurantId}/dishes/{dishId}`
- `DELETE /api/restaurants/{restaurantId}/dishes`

All dish endpoints require authenticated user.

## Identity (custom controller endpoints)

- `PATCH /api/identity/user`
  - Auth: authenticated user
- `POST /api/identity/userRole`
  - Auth: role `Admin`
- `DELETE /api/identity/userRole`
  - Auth: role `Admin`

## Identity (framework mapped endpoints)

Identity endpoints are also mapped via:

- `app.MapGroup("api/identity").MapIdentityApi<User>()`

Check Swagger UI in development for the generated endpoint list and request/response contracts.

