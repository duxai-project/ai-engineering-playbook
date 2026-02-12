# ADR-002: Caching Strategy for AI Responses

## Status
Accepted

## Context

AI API calls are expensive (both in cost and latency). Many queries are repetitive or similar, especially for:
- Common code review patterns
- FAQ-style questions
- Documentation summarization
- Repeated user queries

We need a caching strategy to:
- Reduce API costs (currently $0.03 per 1K tokens for GPT-4)
- Improve response times (2000ms → 200ms)
- Maintain service availability during AI provider outages
- Balance freshness with efficiency

## Decision

Implement a **multi-tier caching strategy**:

1. **L1: In-Memory Cache** (5 min TTL)
   - Fast access for hot queries
   - Limited to 100 MB
   - Per-instance cache

2. **L2: Distributed Cache (Redis)** (24 hr TTL)
   - Shared across instances
   - Longer retention
   - Handles cache invalidation

3. **Cache Key Strategy**
   - Hash of prompt + model + temperature
   - Enables exact match lookups
   - Privacy-preserving (no PII in keys)

### Cache Decision Logic

```csharp
string cacheKey = ComputeCacheKey(prompt, model, temperature);

// L1: Check memory cache
if (memoryCache.TryGetValue(cacheKey, out string result))
    return result;

// L2: Check distributed cache
result = await distributedCache.GetStringAsync(cacheKey);
if (result != null)
{
    memoryCache.Set(cacheKey, result, TimeSpan.FromMinutes(5));
    return result;
}

// L3: Call AI API
result = await aiService.GenerateCompletionAsync(prompt);

// Store in both caches
await distributedCache.SetStringAsync(cacheKey, result, new() { 
    AbsoluteExpirationRelativeToNow = TimeSpan.FromHours(24) 
});
memoryCache.Set(cacheKey, result, TimeSpan.FromMinutes(5));

return result;
```

## Consequences

### Positive

- **Cost Savings**: Projected 60% reduction in AI API costs
- **Performance**: 10x faster for cached responses
- **Availability**: Degraded service during outages instead of total failure
- **Scalability**: Shared cache across instances

### Negative

- **Stale Data**: Cached responses may become outdated
- **Infrastructure Cost**: Redis hosting adds ~$50/month
- **Complexity**: Additional failure mode (cache unavailable)
- **Memory Usage**: Increased memory footprint

### Metrics to Track

- Cache hit rate (target: >50%)
- Average response time (cached vs uncached)
- Cost savings vs cache infrastructure cost
- Cache size and eviction rate

## Alternatives Considered

### Alternative 1: No Caching
Always call AI API for fresh responses.

**Rejected because**: 
- Unsustainable costs at scale
- Poor user experience (slow responses)

### Alternative 2: Database Caching
Store responses in PostgreSQL.

**Rejected because**: 
- Slower than Redis
- Overkill for temporary data

### Alternative 3: Client-Side Caching
Cache in browser/client.

**Considered for**: 
- Future enhancement for user-specific queries
- Not sufficient as primary strategy

## Implementation Notes

### Cache Invalidation Strategy

- TTL-based expiration (primary)
- Manual invalidation via admin API
- Tag-based invalidation for related content

### Cache Warming

For known common queries:
```csharp
await CacheWarmupService.WarmCommonQueriesAsync();
```

### Monitoring

```csharp
metrics.RecordCacheHit("ai_response", hitOrMiss);
metrics.RecordLatency("ai_response", duration, cached);
```

### Security Considerations

- Don't cache responses containing PII
- Implement cache key namespacing per tenant
- Encrypt cached data at rest (Redis TLS)

## Migration Plan

1. Deploy Redis infrastructure
2. Deploy code with caching (feature flag OFF)
3. Enable for 10% of traffic
4. Monitor metrics for 48 hours
5. Gradually increase to 100%

## Success Criteria

- Cache hit rate > 50% within 2 weeks
- Average response time < 500ms
- No increase in error rate
- Cost reduction > 40%

## References

- [Caching Best Practices](https://learn.microsoft.com/en-us/azure/architecture/best-practices/caching)
- [Redis Best Practices](https://redis.io/docs/manual/patterns/)
- [Cache-Aside Pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/cache-aside)

## Date
2024-01-20

## Author
[Your Name]

## Updates

- 2024-02-01: Achieved 65% cache hit rate
- 2024-02-15: Reduced L2 TTL from 48h to 24h based on freshness feedback
