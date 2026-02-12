# Minimal API Patterns

## Objective

Explore modern .NET minimal API patterns for building lightweight, high-performance web APIs.

## Overview

Minimal APIs, introduced in .NET 6 and enhanced in .NET 8, provide a simplified approach to building HTTP APIs with minimal ceremony.

## Core Patterns

### 1. Basic Endpoint Definition

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/api/health", () => Results.Ok(new { status = "healthy" }));
app.MapGet("/api/products/{id}", (int id) => Results.Ok(new { id, name = "Product" }));
app.MapPost("/api/products", (Product product) => Results.Created($"/api/products/{product.Id}", product));

app.Run();
```

### 2. Dependency Injection

```csharp
// Register services
builder.Services.AddScoped<IProductService, ProductService>();

// Use in endpoints
app.MapGet("/api/products", async (IProductService service) =>
{
    var products = await service.GetAllAsync();
    return Results.Ok(products);
});
```

### 3. Route Groups

```csharp
var products = app.MapGroup("/api/products")
    .WithTags("Products")
    .RequireAuthorization();

products.MapGet("/", async (IProductService service) =>
    await service.GetAllAsync());

products.MapGet("/{id}", async (int id, IProductService service) =>
{
    var product = await service.GetByIdAsync(id);
    return product is not null ? Results.Ok(product) : Results.NotFound();
});

products.MapPost("/", async (Product product, IProductService service) =>
{
    await service.CreateAsync(product);
    return Results.Created($"/api/products/{product.Id}", product);
});

products.MapPut("/{id}", async (int id, Product product, IProductService service) =>
{
    if (id != product.Id) return Results.BadRequest();
    await service.UpdateAsync(product);
    return Results.NoContent();
});

products.MapDelete("/{id}", async (int id, IProductService service) =>
{
    await service.DeleteAsync(id);
    return Results.NoContent();
});
```

### 4. Filters and Middleware

```csharp
// Custom endpoint filter
public class ValidationFilter : IEndpointFilter
{
    public async ValueTask<object?> InvokeAsync(
        EndpointFilterInvocationContext context,
        EndpointFilterDelegate next)
    {
        var product = context.GetArgument<Product>(0);
        
        if (string.IsNullOrWhiteSpace(product.Name))
        {
            return Results.ValidationProblem(new Dictionary<string, string[]>
            {
                { "name", new[] { "Name is required" } }
            });
        }
        
        return await next(context);
    }
}

// Apply filter
products.MapPost("/", async (Product product, IProductService service) =>
{
    await service.CreateAsync(product);
    return Results.Created($"/api/products/{product.Id}", product);
})
.AddEndpointFilter<ValidationFilter>();
```

### 5. Error Handling

```csharp
app.UseExceptionHandler(exceptionHandlerApp =>
{
    exceptionHandlerApp.Run(async context =>
    {
        var exception = context.Features.Get<IExceptionHandlerFeature>()?.Error;
        
        var problemDetails = new ProblemDetails
        {
            Status = StatusCodes.Status500InternalServerError,
            Title = "An error occurred",
            Detail = exception?.Message
        };
        
        context.Response.StatusCode = StatusCodes.Status500InternalServerError;
        await context.Response.WriteAsJsonAsync(problemDetails);
    });
});
```

## Advanced Patterns

### OpenAPI/Swagger Integration

```csharp
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

app.UseSwagger();
app.UseSwaggerUI();

// Document endpoints
app.MapGet("/api/products", async (IProductService service) =>
    await service.GetAllAsync())
    .WithName("GetProducts")
    .WithOpenApi(operation =>
    {
        operation.Summary = "Retrieve all products";
        operation.Description = "Returns a list of all available products";
        return operation;
    });
```

### Response Caching

```csharp
builder.Services.AddOutputCache(options =>
{
    options.AddBasePolicy(builder => builder.Expire(TimeSpan.FromMinutes(5)));
    options.AddPolicy("ProductsCache", builder => 
        builder.Expire(TimeSpan.FromMinutes(10)).Tag("products"));
});

app.UseOutputCache();

app.MapGet("/api/products", async (IProductService service) =>
    await service.GetAllAsync())
    .CacheOutput("ProductsCache");
```

### Rate Limiting

```csharp
builder.Services.AddRateLimiter(options =>
{
    options.GlobalLimiter = PartitionedRateLimiter.Create<HttpContext, string>(context =>
        RateLimitPartition.GetFixedWindowLimiter(
            partitionKey: context.User.Identity?.Name ?? context.Request.Headers.Host.ToString(),
            factory: partition => new FixedWindowRateLimiterOptions
            {
                AutoReplenishment = true,
                PermitLimit = 100,
                QueueLimit = 0,
                Window = TimeSpan.FromMinutes(1)
            }));
});

app.UseRateLimiter();
```

## Complete Example: Production-Ready API

```csharp
using Microsoft.AspNetCore.Diagnostics;
using Microsoft.AspNetCore.Mvc;

var builder = WebApplication.CreateBuilder(args);

// Configure services
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();
builder.Services.AddScoped<IProductService, ProductService>();
builder.Services.AddOutputCache();
builder.Services.AddRateLimiter(/* configuration */);

// Configure logging
builder.Logging.AddConsole();
builder.Logging.AddApplicationInsights();

var app = builder.Build();

// Configure middleware pipeline
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();
app.UseRateLimiter();
app.UseOutputCache();

// Global error handling
app.UseExceptionHandler(/* configuration */);

// Define API routes
var api = app.MapGroup("/api")
    .WithOpenApi();

var products = api.MapGroup("/products")
    .WithTags("Products");

products.MapGet("/", GetProducts)
    .CacheOutput();

products.MapGet("/{id}", GetProduct);

products.MapPost("/", CreateProduct)
    .AddEndpointFilter<ValidationFilter>();

products.MapPut("/{id}", UpdateProduct);

products.MapDelete("/{id}", DeleteProduct);

app.Run();

// Endpoint handlers
static async Task<IResult> GetProducts(IProductService service)
{
    var products = await service.GetAllAsync();
    return Results.Ok(products);
}

static async Task<IResult> GetProduct(int id, IProductService service)
{
    var product = await service.GetByIdAsync(id);
    return product is not null ? Results.Ok(product) : Results.NotFound();
}

static async Task<IResult> CreateProduct(Product product, IProductService service)
{
    await service.CreateAsync(product);
    return Results.Created($"/api/products/{product.Id}", product);
}

static async Task<IResult> UpdateProduct(int id, Product product, IProductService service)
{
    if (id != product.Id) return Results.BadRequest();
    var exists = await service.GetByIdAsync(id);
    if (exists is null) return Results.NotFound();
    
    await service.UpdateAsync(product);
    return Results.NoContent();
}

static async Task<IResult> DeleteProduct(int id, IProductService service)
{
    var exists = await service.GetByIdAsync(id);
    if (exists is null) return Results.NotFound();
    
    await service.DeleteAsync(id);
    return Results.NoContent();
}

record Product(int Id, string Name, decimal Price);
```

## Results & Learnings

### Performance Benefits
- **~30% faster startup** compared to controller-based APIs
- **Lower memory footprint**: Minimal overhead
- **Better throughput**: Optimized request pipeline

### Key Findings
1. **Simplicity**: Less boilerplate, more focused on business logic
2. **Performance**: Native AOT compatibility for even faster cold starts
3. **Testability**: Easy to test with WebApplicationFactory
4. **Flexibility**: Can mix with controllers when needed

## Best Practices

1. **Use Route Groups**: Organize related endpoints
2. **Apply Filters**: Centralize cross-cutting concerns
3. **Document APIs**: Use OpenAPI/Swagger annotations
4. **Handle Errors**: Implement global error handling
5. **Validate Input**: Use endpoint filters for validation
6. **Cache Responses**: Leverage output caching for performance
7. **Rate Limit**: Protect APIs from abuse

## Production Considerations

- **Authentication/Authorization**: Integrate identity providers
- **Logging**: Use structured logging with correlation IDs
- **Health Checks**: Implement /health endpoints
- **Versioning**: Plan API versioning strategy
- **Monitoring**: Integrate Application Insights or similar
- **Testing**: Write integration tests for all endpoints

## Resources

- [Minimal APIs Overview](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis)
- [Minimal APIs Quick Reference](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/min-api-filters)
- [Performance Best Practices](https://learn.microsoft.com/en-us/aspnet/core/performance/performance-best-practices)
