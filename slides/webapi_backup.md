---
marp: true
theme: default
paginate: true
size: 16:9
style: |
  section {
    background-image: url('./img/imagem2.png');
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
  }
  .columns {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1rem;
  }
  .small-text {
    font-size: 0.8em;
  }
  .code-highlight {
    background-color: #f8f9fa;
    border-radius: 8px;
    padding: 1rem;
    margin: 0.5rem 0;
  }

---

![bg cover](./img/Imagem1.png)

# ASP.NET Core Web API

### Construindo APIs Modernas e Performáticas

*Guia Introdutório ao Desenvolvimento de Web APIs com ASP.NET Core*

---

## O que é ASP.NET Core Web API?

- **Framework para criação de APIs REST** e HTTP services
- **Parte do ecossistema ASP.NET Core**
- **Cross-platform**: Windows, Linux, macOS
- **High-performance** e escalável
- **Integração nativa** com dependency injection, logging, configuration

### Características Principais
✅ Lightweight e modular  
✅ Built-in JSON serialization  
✅ Automatic model binding  
✅ Comprehensive routing system

---

## Por que ASP.NET Core Web API?

<div class="columns">
<div>

### 🚀 **Performance**
- Um dos frameworks mais rápidos
- Minimal overhead
- Async/await nativo


### 🔧 **Produtividade**
- Scaffolding automático
- Hot reload
- Rich tooling ecosystem

</div>
<div>

### 🛠️ **Ferramentas**
- Visual Studio
- Visual Studio Code
- JetBrains Rider

</div>
</div>

---

## Evolução das Web APIs


```csharp
public class ValuesController : ApiController
{
    public IEnumerable<string> Get()
    {
        return new string[] { "value1", "value2" };
    }
}
```

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

---

## Minimal APIs vs Controller-based APIs

<div class="columns">
<div>

### **Minimal APIs** (.NET 6+)
```csharp
var app = WebApplication.Create();

app.MapGet("/products", 
    () => Products.GetAll());

app.MapPost("/products", 
    (Product product) => 
        Products.Create(product));
app.Run();
```

**Ideal para:**
- APIs simples, microservices e Prototipagem rápida

</div>
    <div>

### **Controller-based APIs**
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

**Ideal para:**  APIs complexas,  Múltiplas actions e melhor organização

</div>
</div>

---

## Estrutura de um Projeto Web API

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
- **Program.cs**: Configuração e startup
- **appsettings.json**: Configurações da aplicação
- **Controllers**: Endpoints da API

---

## Criando sua Primeira Web API

### 1. Criando o Projeto
```bash
dotnet new webapi -n MyFirstAPI
cd MyFirstAPI
dotnet run
```

### 2. Estrutura Inicial (Program.cs)
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

---

## Controllers e Actions

### Controller Básico
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

    
### 🌐 **Modernidade**
- OpenAPI/Swagger integrado
- JWT Authentication
- CORS support
- Rate limiting

### 🔒 **Segurança**
- Authentication & Authorization
- Data protection APIs
- HTTPS por padrão
- Input validation

</div>
</div>

---

## Evolução das Web APIs

### ASP.NET Framework (Legacy)
```csharp
public class ValuesController : ApiController
{
    public IEnumerable<string> Get()
    {
        return new string[] { "value1", "value2" };
    }
}
```

### ASP.NET Core (Moderno)
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

---

## Minimal APIs vs Controller-based APIs

<div class="columns">
<div>

### **Minimal APIs** (.NET 6+)
```csharp
var app = WebApplication.Create();

app.MapGet("/products", 
    () => Products.GetAll());

app.MapPost("/products", 
    (Product product) => 
        Products.Create(product));

app.Run();
```

**Ideal para:**
- APIs simples
- Microservices
- Prototipagem rápida

</div>
<div>

### **Controller-based APIs**
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

**Ideal para:**
- APIs complexas
- Múltiplas actions
- Melhor organização

</div>
</div>

---

## Estrutura de um Projeto Web API

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
- **Program.cs**: Configuração e startup
- **appsettings.json**: Configurações da aplicação
- **Controllers**: Endpoints da API

---

## Criando sua Primeira Web API

### 1. Criando o Projeto
```bash
dotnet new webapi -n MyFirstAPI
cd MyFirstAPI
dotnet run
```

### 2. Estrutura Inicial (Program.cs)
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

---

## Controllers e Actions

### Controller Básico
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

---

## HTTP Verbs e Status Codes

<div class="columns">
<div>

### **HTTP Verbs**
- **GET**: Recuperar dados
- **POST**: Criar recursos
- **PUT**: Atualizar (substituir)
- **PATCH**: Atualizar (parcial)
- **DELETE**: Remover recursos

### **Action Results**
- `Ok(data)` → 200
- `Created()` → 201
- `NoContent()` → 204
- `BadRequest()` → 400
- `NotFound()` → 404

</div>
<div>

### **Exemplo Completo**
```csharp
[HttpPost]
public async Task<IActionResult> CreateProduct(
    [FromBody] CreateProductRequest request)
{
    if (!ModelState.IsValid)
        return BadRequest(ModelState);
    
    var product = await _service
        .CreateAsync(request);
    
    return CreatedAtAction(
        nameof(GetProduct),
        new { id = product.Id },
        product);
}
```

</div>
</div>

---

## Model Binding e Validation

### Data Annotations
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

---

## Dependency Injection

### Registrando Serviços
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

---

## Entity Framework Core Integration

### Model
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

---

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

---

## Authentication & Authorization

### JWT Configuration
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

---

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

---

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
        
        var response = new { message = "An error occurred", detail = exception.Message };
        await context.Response.WriteAsync(JsonSerializer.Serialize(response));
    }
}
```

---

## Performance & Best Practices

<div class="columns">
<div>

### **Performance Tips**
- Use `async/await` consistentemente
- Implemente paginação
- Use response caching
- Otimize queries do EF Core
- Implemente connection pooling

### **Caching**
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

</div>
<div>

### **Security Best Practices**
- Sempre use HTTPS
- Validação de input rigorosa
- Rate limiting
- CORS apropriado
- Não exponha informações sensíveis

### **API Versioning**
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

</div>
</div>

---

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

---

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

---

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

---

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

## Exemplo Completo - Controller

```csharp
[ApiController]
[Route("api/[controller]")]
[Produces("application/json")]
public class ProductsController : ControllerBase
{
    private readonly IProductService _productService;
    private readonly ILogger<ProductsController> _logger;
    
    public ProductsController(IProductService productService, 
                             ILogger<ProductsController> logger)
    {
        _productService = productService;
        _logger = logger;
    }
    
    /// <summary>
    /// Retorna todos os produtos ativos
    /// </summary>
    [HttpGet]
    [ProducesResponseType(typeof(IEnumerable<ProductDto>), 200)]
    public async Task<ActionResult<IEnumerable<ProductDto>>> GetProducts(
        [FromQuery] int page = 1, 
        [FromQuery] int pageSize = 10,
        [FromQuery] string search = "")
    {
        var products = await _productService.GetProductsAsync(page, pageSize, search);
        return Ok(products);
    }
    
    /// <summary>
    /// Retorna um produto específico
    /// </summary>
    [HttpGet("{id}")]
    [ProducesResponseType(typeof(ProductDto), 200)]
    [ProducesResponseType(404)]
    public async Task<ActionResult<ProductDto>> GetProduct(int id)
    {
        var product = await _productService.GetProductByIdAsync(id);
        return product == null ? NotFound() : Ok(product);
    }
}
```

---

## Roadmap e Futuro

### **Novas Features (.NET 9+)**
- **Native AOT support** melhorado
- **Minimal APIs** com mais recursos
- **gRPC improvements**
- **Cloud-native optimizations**

### **Tendências**
- **GraphQL integration**
- **Real-time APIs** com SignalR
- **AI/ML integration**
- **Serverless optimizations**

### **Best Practices Emergentes**
- Clean Architecture
- CQRS com MediatR
- Event-driven architecture
- Microservices patterns

---

## Recursos para Aprender Mais

<div class="columns">
<div>

### 📚 **Documentação Oficial**
- [ASP.NET Core Documentation](https://docs.microsoft.com/aspnet/core)
- [Web API Tutorial](https://docs.microsoft.com/aspnet/core/tutorials/web-api)
- [EF Core Guide](https://docs.microsoft.com/ef/core)

### 🎥 **Vídeos e Cursos**
- Microsoft Learn
- Pluralsight ASP.NET Core Path
- YouTube: .NET Channel

</div>
<div>

### 🛠️ **Ferramentas Úteis**
- Postman/Insomnia (API testing)
- Swagger/OpenAPI (Documentation)
- EF Core Power Tools
- dotnet CLI templates

### 👥 **Comunidade**
- Stack Overflow
- Reddit: r/dotnet
- Discord: .NET Community
- GitHub Discussions

</div>
</div>

---

## Conclusão

### ASP.NET Core Web API é a escolha ideal para:

#### ✨ **APIs Modernas e Performáticas**
- Framework maduro e bem documentado
- Performance excepcional
- Ecossistema rico e ativo
- Suporte empresarial robusto

#### 🚀 **Desenvolvimento Produtivo**
- Tooling de primeira classe
- Integração nativa com Azure
- Padrões de mercado estabelecidos
- Community support forte

---

# Obrigado!

### Perguntas?

**Próximos passos:**
- Crie sua primeira Web API
- Explore os templates do .NET CLI
- Pratique com projetos reais

**Recursos úteis:**
- [dotnet new webapi](https://docs.microsoft.com/dotnet/core/tools/dotnet-new)
- [ASP.NET Core Samples](https://github.com/dotnet/AspNetCore.Docs)

*Vamos construir APIs incríveis com ASP.NET Core!* 🚀