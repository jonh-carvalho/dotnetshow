# Guia Prático: Criando sua Primeira Web API com ASP.NET Core

**Documento Técnico - Desenvolvimento .NET**  
*Versão 1.0 - Setembro 2025*

---

## Índice

1. [Criando o Projeto](#criando-o-projeto)
2. [Estrutura do Projeto](#estrutura-do-projeto)
3. [Controllers e Actions](#controllers-e-actions)
4. [HTTP Verbs e Status Codes](#http-verbs-e-status-codes)
5. [Model Binding e Validation](#model-binding-e-validation)
6. [Dependency Injection](#dependency-injection)
7. [Entity Framework Core](#entity-framework-core-integration)
8. [Repository Pattern](#repository-pattern)
9. [Authentication & Authorization](#authentication--authorization)
10. [Swagger/OpenAPI Documentation](#swaggeropenapi-documentation)
11. [Error Handling & Middleware](#error-handling--middleware)
12. [Performance & Best Practices](#performance--best-practices)
13. [Testing Web APIs](#testing-web-apis)
14. [Deployment & DevOps](#deployment--devops)
15. [Monitoramento e Observabilidade](#monitoramento-e-observabilidade)
16. [Exemplo Completo](#exemplo-completo---e-commerce-api)

---

## Criando o Projeto

### 1. Comando de Criação

Para criar um novo projeto Web API, execute os seguintes comandos no terminal:

```bash
dotnet new webapi -n MyFirstAPI --framework net8.0
cd MyFirstAPI
dotnet run
```
> **Dica:** Para garantir que está usando o SDK .NET 8 LTS, execute `dotnet --version` e verifique se retorna `8.x.x`. Caso tenha múltiplos SDKs instalados, utilize um arquivo `global.json`:

```json
{
    "sdk": {
        "version": "8.0.100"
    }
}
```
```

### 2. Estrutura Inicial (Program.cs)

O arquivo `Program.cs` é o ponto de entrada da aplicação. Configure-o da seguinte forma:

```csharp
var builder = WebApplication.CreateBuilder(args);

// Add services
builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// Configure pipeline
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();
app.MapControllers();
app.Run();
```

## Estrutura do Projeto

Um projeto Web API bem organizado deve seguir a seguinte estrutura de diretórios:

```
MyWebAPI/
├── Controllers/          # API Controllers
├── Models/              # Data models/DTOs
├── Services/            # Business logic
├── Data/               # Data access layer
├── Middleware/         # Custom middleware
├── Extensions/         # Extension methods
├── appsettings.json    # Configuration
└── Program.cs          # Application entry point
```

### Principais Arquivos

- **Program.cs**: Configuração e startup da aplicação
- **appsettings.json**: Configurações da aplicação (strings de conexão, configurações de ambiente)
- **Controllers**: Contém os endpoints da API que respondem às requisições HTTP

## Controllers e Actions

### Controller Básico

Um controller é uma classe que herda de `ControllerBase` e contém as actions (métodos) que respondem às requisições HTTP:

```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    private readonly IProductService _productService;
    
    public ProductsController(IProductService productService)
    {
        _productService = productService;
    }
    
    [HttpGet]
    public async Task<ActionResult<IEnumerable<Product>>> GetProducts()
    {
        var products = await _productService.GetAllAsync();
        return Ok(products);
    }
    
    [HttpGet("{id}")]
    public async Task<ActionResult<Product>> GetProduct(int id)
    {
        var product = await _productService.GetByIdAsync(id);
        return product == null ? NotFound() : Ok(product);
    }
}
```

### Características Modernas vs Legacy

**ASP.NET Core (Moderno):**
```csharp
[ApiController]
[Route("api/[controller]")]
public class ValuesController : ControllerBase
{
    [HttpGet]
    public ActionResult<IEnumerable<string>> Get()
    {
        return Ok(new[] { "value1", "value2" });
    }
}
```

**ASP.NET Framework (Legacy):**
```csharp
public class ValuesController : ApiController
{
    public IEnumerable<string> Get()
    {
        return new string[] { "value1", "value2" };
    }
}
```

### Minimal APIs vs Controller-based APIs

#### Minimal APIs (.NET 6+)

Ideal para APIs simples, microservices e prototipagem rápida:

```csharp
var app = WebApplication.Create();

app.MapGet("/products", () => Products.GetAll());

app.MapPost("/products", (Product product) => 
    Products.Create(product));

app.Run();
```

#### Controller-based APIs

Ideal para APIs complexas, múltiplas actions e melhor organização:

```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController 
{
    [HttpGet]
    public IActionResult GetAll()
        => Ok(Products.GetAll());
    
    [HttpPost]
    public IActionResult Create(Product p)
        => CreatedAtAction(nameof(Get), 
            new { id = p.Id }, p);
}
```

## HTTP Verbs e Status Codes

### HTTP Verbs

- **GET**: Recuperar dados
- **POST**: Criar recursos
- **PUT**: Atualizar (substituir completamente)
- **PATCH**: Atualizar (parcial)
- **DELETE**: Remover recursos

### Action Results Comuns

- `Ok(data)` → 200 (Success)
- `Created()` → 201 (Created)
- `NoContent()` → 204 (No Content)
- `BadRequest()` → 400 (Bad Request)
- `NotFound()` → 404 (Not Found)

### Exemplo Completo

```csharp
[HttpPost]
public async Task<IActionResult> CreateProduct(
    [FromBody] CreateProductRequest request)
{
    if (!ModelState.IsValid)
        return BadRequest(ModelState);
    
    var product = await _service.CreateAsync(request);
    
    return CreatedAtAction(
        nameof(GetProduct),
        new { id = product.Id },
        product);
}
```

## Model Binding e Validation

### Data Annotations

Use Data Annotations para validar automaticamente os dados de entrada:

```csharp
public class CreateProductRequest
{
    [Required(ErrorMessage = "Nome é obrigatório")]
    [StringLength(100, MinimumLength = 2)]
    public string Name { get; set; }
    
    [Range(0.01, double.MaxValue, 
           ErrorMessage = "Preço deve ser maior que zero")]
    public decimal Price { get; set; }
    
    [EmailAddress]
    public string ContactEmail { get; set; }
    
    [Url]
    public string Website { get; set; }
}
```

### Custom Validation

```csharp
public class ProductController : ControllerBase
{
    [HttpPost]
    public IActionResult Create([FromBody] CreateProductRequest request)
    {
        if (!ModelState.IsValid)
            return BadRequest(ModelState);
        
        // Business logic here...
        return CreatedAtAction(nameof(GetProduct), product);
    }
}
```

## Dependency Injection

### Registrando Serviços

Configure os serviços no arquivo `Program.cs`:

```csharp
// Program.cs
builder.Services.AddScoped<IProductService, ProductService>();
builder.Services.AddScoped<IRepository<Product>, ProductRepository>();
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(connectionString));

// Singleton para cache
builder.Services.AddSingleton<ICacheService, MemoryCacheService>();

// Transient para helpers
builder.Services.AddTransient<IEmailService, EmailService>();
```

### Injetando no Controller

```csharp
[ApiController]
public class ProductsController : ControllerBase
{
    private readonly IProductService _productService;
    private readonly ILogger<ProductsController> _logger;
    
    public ProductsController(
        IProductService productService,
        ILogger<ProductsController> logger)
    {
        _productService = productService;
        _logger = logger;
    }
}
```

## Entity Framework Core Integration

### Model

Defina seus modelos de dados:

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
    public DateTime CreatedAt { get; set; }
    public bool IsActive { get; set; }
    
    // Navigation properties
    public int CategoryId { get; set; }
    public Category Category { get; set; }
}
```

### DbContext

Configure o contexto do banco de dados:

```csharp
public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options) { }
    
    public DbSet<Product> Products { get; set; }
    public DbSet<Category> Categories { get; set; }
    
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Product>(entity =>
        {
            entity.Property(e => e.Price).HasPrecision(18, 2);
            entity.HasIndex(e => e.Name).IsUnique();
        });
    }
}
```

## Repository Pattern

### Interface

```csharp
public interface IRepository<T> where T : class
{
    Task<IEnumerable<T>> GetAllAsync();
    Task<T> GetByIdAsync(int id);
    Task<T> CreateAsync(T entity);
    Task<T> UpdateAsync(T entity);
    Task DeleteAsync(int id);
}
```

### Implementação

```csharp
public class ProductRepository : IRepository<Product>
{
    private readonly ApplicationDbContext _context;
    
    public ProductRepository(ApplicationDbContext context)
    {
        _context = context;
    }
    
    public async Task<IEnumerable<Product>> GetAllAsync()
    {
        return await _context.Products
            .Include(p => p.Category)
            .Where(p => p.IsActive)
            .ToListAsync();
    }
    
    public async Task<Product> GetByIdAsync(int id)
    {
        return await _context.Products
            .Include(p => p.Category)
            .FirstOrDefaultAsync(p => p.Id == id);
    }
}
```

## Authentication & Authorization

### JWT Configuration

Configure autenticação JWT no `Program.cs`:

```csharp
// Program.cs
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateIssuerSigningKey = true,
            IssuerSigningKey = new SymmetricSecurityKey(key),
            ValidateIssuer = true,
            ValidIssuer = "your-issuer",
            ValidateAudience = true,
            ValidAudience = "your-audience",
            ValidateLifetime = true,
            ClockSkew = TimeSpan.Zero
        };
    });

builder.Services.AddAuthorization();
```

### Protecting Endpoints

```csharp
[ApiController]
[Route("api/[controller]")]
[Authorize] // Requires authentication for all actions
public class ProductsController : ControllerBase
{
    [HttpGet]
    [AllowAnonymous] // Public endpoint
    public IActionResult GetPublicProducts() => Ok();
    
    [HttpPost]
    [Authorize(Roles = "Admin")] // Requires Admin role
    public IActionResult CreateProduct() => Ok();
    
    [HttpDelete("{id}")]
    [Authorize(Policy = "CanDeleteProducts")] // Custom policy
    public IActionResult DeleteProduct(int id) => Ok();
}
```

## Swagger/OpenAPI Documentation

### Configuração Básica

```csharp
builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo
    {
        Title = "My API",
        Version = "v1",
        Description = "Uma API para gerenciar produtos",
        Contact = new OpenApiContact
        {
            Name = "Equipe de Desenvolvimento",
            Email = "dev@empresa.com"
        }
    });
    
    // JWT Authentication
    c.AddSecurityDefinition("Bearer", new OpenApiSecurityScheme
    {
        Description = "JWT Authorization header",
        Name = "Authorization",
        In = ParameterLocation.Header,
        Type = SecuritySchemeType.ApiKey
    });
});
```

### Documentando Endpoints

```csharp
[HttpGet("{id}")]
[ProducesResponseType(typeof(Product), StatusCodes.Status200OK)]
[ProducesResponseType(StatusCodes.Status404NotFound)]
[ProducesResponseType(StatusCodes.Status400BadRequest)]
public async Task<IActionResult> GetProduct(
    /// <summary>ID do produto</summary>
    /// <param name="id">Identificador único do produto</param>
    int id)
{
    // Implementation...
}
```

## Error Handling & Middleware

### Global Exception Handler

```csharp
public class GlobalExceptionMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<GlobalExceptionMiddleware> _logger;
    
    public GlobalExceptionMiddleware(RequestDelegate next, 
                                   ILogger<GlobalExceptionMiddleware> logger)
    {
        _next = next;
        _logger = logger;
    }
    
    public async Task InvokeAsync(HttpContext context)
    {
        try
        {
            await _next(context);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "An unhandled exception occurred");
            await HandleExceptionAsync(context, ex);
        }
    }
    
    private static async Task HandleExceptionAsync(HttpContext context, 
                                                  Exception exception)
    {
        context.Response.ContentType = "application/json";
        context.Response.StatusCode = StatusCodes.Status500InternalServerError;
        
        var response = new { 
            message = "An error occurred", 
            detail = exception.Message 
        };
        await context.Response.WriteAsync(JsonSerializer.Serialize(response));
    }
}
```

## Performance & Best Practices

### Performance Tips

- Use `async/await` consistentemente
- Implemente paginação para grandes datasets
- Use response caching quando apropriado
- Otimize queries do Entity Framework Core
- Implemente connection pooling

### Exemplo de Caching

```csharp
[HttpGet]
[ResponseCache(Duration = 300)]
public IActionResult GetProducts()
{
    return Ok(_cache.GetOrCreate(
        "products", 
        factory => GetProductsFromDb()));
}
```

### Security Best Practices

- Sempre use HTTPS em produção
- Implemente validação de input rigorosa
- Configure rate limiting
- Configure CORS apropriadamente
- Não exponha informações sensíveis nos responses

### API Versioning

```csharp
[ApiController]
[Route("api/v{version:apiVersion}/products")]
[ApiVersion("1.0")]
[ApiVersion("2.0")]
public class ProductsController : ControllerBase
{
    [HttpGet]
    [MapToApiVersion("1.0")]
    public IActionResult GetV1() => Ok("V1");
    
    [HttpGet]
    [MapToApiVersion("2.0")]
    public IActionResult GetV2() => Ok("V2");
}
```

## Testing Web APIs

### Unit Testing

```csharp
[Test]
public async Task GetProduct_WithValidId_ReturnsProduct()
{
    // Arrange
    var mockService = new Mock<IProductService>();
    var expectedProduct = new Product { Id = 1, Name = "Test" };
    mockService.Setup(s => s.GetByIdAsync(1))
           .ReturnsAsync(expectedProduct);
    
    var controller = new ProductsController(mockService.Object);
    
    // Act
    var result = await controller.GetProduct(1);
    
    // Assert
    var okResult = result.Result as OkObjectResult;
    Assert.That(okResult?.Value, Is.EqualTo(expectedProduct));
}
```

### Integration Testing

```csharp
public class ProductsControllerIntegrationTests : IClassFixture<WebApplicationFactory<Program>>
{
    private readonly WebApplicationFactory<Program> _factory;
    
    [Test]
    public async Task GetProducts_ReturnsSuccessAndCorrectContentType()
    {
        var client = _factory.CreateClient();
        var response = await client.GetAsync("/api/products");
        
        response.EnsureSuccessStatusCode();
        Assert.That(response.Content.Headers.ContentType?.ToString(),
                   Is.EqualTo("application/json; charset=utf-8"));
    }
}
```

## Deployment & DevOps

### Docker Configuration

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["MyAPI.csproj", "."]
RUN dotnet restore
COPY . .
RUN dotnet build -c Release -o /app/build

FROM build AS publish
RUN dotnet publish -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MyAPI.dll"]
```

### Azure App Service

```json
{
  "version": 2,
  "builds": [
    {
      "src": "*.csproj",
      "use": "@vercel/dotnet"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "/"
    }
  ]
}
```

## Monitoramento e Observabilidade

### Logging

```csharp
[HttpGet("{id}")]
public async Task<IActionResult> GetProduct(int id)
{
    _logger.LogInformation("Fetching product with ID: {ProductId}", id);
    
    try
    {
        var product = await _productService.GetByIdAsync(id);
        if (product == null)
        {
            _logger.LogWarning("Product with ID {ProductId} not found", id);
            return NotFound();
        }
        
        _logger.LogInformation("Successfully retrieved product {ProductId}", id);
        return Ok(product);
    }
    catch (Exception ex)
    {
        _logger.LogError(ex, "Error fetching product {ProductId}", id);
        throw;
    }
}
```

### Health Checks

```csharp
builder.Services.AddHealthChecks()
    .AddDbContext<ApplicationDbContext>()
    .AddUrlGroup(new Uri("https://external-api.com/health"), "External API");

app.MapHealthChecks("/health");
```

## Exemplo Completo - E-commerce API

### Product Model

```csharp
public class Product
{
    public int Id { get; set; }
    
    [Required, StringLength(100)]
    public string Name { get; set; }
    
    [StringLength(500)]
    public string Description { get; set; }
    
    [Range(0.01, double.MaxValue)]
    public decimal Price { get; set; }
    
    public int Stock { get; set; }
    public bool IsActive { get; set; }
    public DateTime CreatedAt { get; set; }
    
    public int CategoryId { get; set; }
    public Category Category { get; set; }
}

public class Category
{
    public int Id { get; set; }
    public string Name { get; set; }
    public ICollection<Product> Products { get; set; }
}
```

---

## Conclusão

Este guia apresentou os conceitos fundamentais para criar uma Web API moderna com ASP.NET Core. A partir desta base, você pode expandir sua aplicação com recursos mais avançados como autenticação, autorização, cache, monitoramento e deploy em produção.

### Próximos Passos Recomendados

1. Implemente autenticação JWT
2. Adicione testes automatizados
3. Configure CI/CD
4. Implemente monitoramento
5. Otimize para produção

### Recursos Adicionais

- [Documentação Oficial ASP.NET Core](https://docs.microsoft.com/aspnet/core)
- [Microsoft Learn - ASP.NET Core](https://learn.microsoft.com)
- [GitHub - Exemplos ASP.NET Core](https://github.com/dotnet/AspNetCore.Docs)

---

*Documento gerado em setembro de 2025 - ASP.NET Core 8.0*