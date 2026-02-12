# Quick Start Guide

Welcome to the AI Engineering Playbook! This guide will help you navigate and get the most out of this repository.

## 🎯 What You'll Find Here

This playbook documents practical experiences, patterns, and best practices for building AI-powered systems with modern .NET.

## 📖 Where to Start

### New to AI Integration?
1. Start with **[AI Experiments](./ai-experiments/README.md)**
   - Begin with [Prompt Engineering](./ai-experiments/01-prompt-engineering/README.md)
   - Then explore [Semantic Kernel Basics](./ai-experiments/02-semantic-kernel-basics/README.md)

### Building .NET APIs with AI?
1. Check out **[.NET Systems](./dotnet-systems/README.md)**
   - Review [Minimal API Patterns](./dotnet-systems/01-minimal-api-patterns/README.md)
   - Learn [AI Integration Patterns](./dotnet-systems/02-ai-integration-patterns/README.md)

### Designing Production Systems?
1. Explore **[System Design](./system-design/README.md)**
   - Read [ADR-001: AI Service Abstraction](./system-design/adr/001-ai-service-abstraction.md)
   - Review [ADR-002: Caching Strategy](./system-design/adr/002-caching-strategy.md)

### Learning from Production Experience?
1. Visit **[Production Notes](./production-notes/README.md)**
   - Study the [Rate Limit Incident Report](./production-notes/incidents/2024-02-10-rate-limit-incident.md)

## 🗺️ Repository Structure

```
ai-engineering-playbook/
│
├── ai-experiments/           # Hands-on AI experiments
│   ├── 01-prompt-engineering/
│   ├── 02-semantic-kernel-basics/
│   └── README.md
│
├── dotnet-systems/          # .NET patterns and practices
│   ├── 01-minimal-api-patterns/
│   ├── 02-ai-integration-patterns/
│   └── README.md
│
├── system-design/           # Architecture decisions
│   ├── adr/                # Architecture Decision Records
│   └── README.md
│
├── production-notes/        # Real-world learnings
│   ├── incidents/          # Incident reports
│   └── README.md
│
├── CONTRIBUTING.md          # Contribution guidelines
└── README.md               # Main overview
```

## 💡 How to Use This Playbook

### For Learning
- Read through examples sequentially
- Try implementing patterns in your projects
- Compare your experiences with documented learnings

### For Reference
- Use as a quick reference for patterns
- Copy and adapt code examples
- Review ADRs when facing similar decisions

### For Sharing
- Reference specific sections in discussions
- Share learnings with your team
- Contribute your own experiences

## 🔑 Key Topics

### AI Integration
- Service abstraction patterns
- Caching strategies
- Resilience and error handling
- Cost optimization
- Prompt management

### .NET Development
- Minimal APIs
- Dependency injection
- Semantic Kernel integration
- Performance optimization

### Production Operations
- Monitoring and observability
- Incident response
- Rate limiting
- Deployment strategies

## 🚀 Quick Examples

### Simple AI Service Integration

```csharp
// 1. Define abstraction
public interface IAIService
{
    Task<string> GenerateCompletionAsync(string prompt);
}

// 2. Register in DI
builder.Services.AddSingleton<IAIService, AzureOpenAIService>();
builder.Services.Decorate<IAIService, CachedAIService>();

// 3. Use in endpoints
app.MapPost("/api/analyze", async (
    [FromBody] CodeRequest request,
    IAIService aiService) =>
{
    var result = await aiService.GenerateCompletionAsync(
        $"Review this code: {request.Code}");
    return Results.Ok(new { analysis = result });
});
```

See [AI Integration Patterns](./dotnet-systems/02-ai-integration-patterns/README.md) for complete examples.

### Implementing Circuit Breaker

```csharp
var pipeline = new ResiliencePipelineBuilder()
    .AddRetry(new RetryStrategyOptions
    {
        MaxRetryAttempts = 3,
        BackoffType = DelayBackoffType.Exponential
    })
    .AddCircuitBreaker(new CircuitBreakerStrategyOptions
    {
        FailureRatio = 0.5,
        BreakDuration = TimeSpan.FromMinutes(1)
    })
    .Build();
```

See [ADR-001](./system-design/adr/001-ai-service-abstraction.md) for architectural context.

## 📚 Recommended Reading Path

1. **Week 1**: AI Fundamentals
   - Prompt Engineering
   - Basic Semantic Kernel usage

2. **Week 2**: .NET Integration
   - Minimal API patterns
   - AI service abstraction

3. **Week 3**: Production Patterns
   - Caching strategies
   - Resilience patterns

4. **Week 4**: System Design
   - Architecture decisions
   - Incident learnings

## 🤝 Contributing

Want to suggest improvements or share your experiences?

1. Read [CONTRIBUTING.md](./CONTRIBUTING.md)
2. Open an issue for discussion
3. Submit a pull request with your changes

## 📞 Getting Help

- **Issues**: For bugs or questions
- **Discussions**: For open-ended topics
- **Pull Requests**: For contributions

## 🔖 Bookmarks

Save these for quick access:

- [AI Experiments](./ai-experiments/README.md)
- [.NET Systems](./dotnet-systems/README.md)
- [System Design](./system-design/README.md)
- [Production Notes](./production-notes/README.md)
- [Contributing Guide](./CONTRIBUTING.md)

---

**Ready to dive in?** Pick a section that interests you and start exploring! 🚀
