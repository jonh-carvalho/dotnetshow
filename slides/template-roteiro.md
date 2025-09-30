---
marp: true
theme: default
paginate: true
style: |
  @import url('https://fonts.googleapis.com/css2?family=Fira+Code:wght@300;400;500&family=Inter:wght@300;400;500;600;700&display=swap');
  
  section {
    font-family: 'Inter', sans-serif;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: #2d3748;
    padding: 60px;
    line-height: 1.6;
  }
  
  h1, h2, h3, h4, h5, h6 {
    color: #1a202c;
    font-weight: 600;
    margin-bottom: 1.5rem;
  }
  
  h1 {
    font-size: 2.5rem;
    color: white;
    text-align: center;
    background: rgba(0,0,0,0.3);
    padding: 2rem;
    border-radius: 15px;
    backdrop-filter: blur(10px);
  }
  
  h2 {
    font-size: 1.8rem;
    color: #2b6cb0;
    border-left: 5px solid #4299e1;
    padding-left: 1rem;
    background: rgba(255,255,255,0.9);
    padding: 1rem;
    border-radius: 8px;
  }
  
  h3 {
    font-size: 1.4rem;
    color: #2c5aa0;
    margin-top: 2rem;
  }
  
  code {
    font-family: 'Fira Code', monospace;
    background: #f7fafc;
    padding: 0.2rem 0.4rem;
    border-radius: 4px;
    border: 1px solid #e2e8f0;
    font-size: 0.9em;
  }
  
  pre {
    background: #1a202c;
    color: #e2e8f0;
    padding: 1.5rem;
    border-radius: 10px;
    overflow-x: auto;
    box-shadow: 0 10px 25px rgba(0,0,0,0.2);
    border-left: 5px solid #4299e1;
  }
  
  pre code {
    background: transparent;
    border: none;
    padding: 0;
    color: inherit;
    font-size: 0.95em;
  }
  
  blockquote {
    border-left: 5px solid #38b2ac;
    background: rgba(56, 178, 172, 0.1);
    padding: 1rem 1.5rem;
    margin: 1.5rem 0;
    border-radius: 5px;
    font-style: italic;
  }
  
  ul, ol {
    padding-left: 1.5rem;
  }
  
  li {
    margin: 0.5rem 0;
  }
  
  table {
    border-collapse: collapse;
    width: 100%;
    margin: 1.5rem 0;
    background: white;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  }
  
  th, td {
    border: 1px solid #e2e8f0;
    padding: 0.75rem;
    text-align: left;
  }
  
  th {
    background: #4299e1;
    color: white;
    font-weight: 600;
  }
  
  .step-number {
    background: #4299e1;
    color: white;
    border-radius: 50%;
    padding: 0.5rem 1rem;
    font-weight: bold;
    display: inline-block;
    margin-right: 1rem;
    margin-bottom: 1rem;
  }
  
  .highlight {
    background: #fef5e7;
    border: 2px solid #f6ad55;
    padding: 1rem;
    border-radius: 8px;
    margin: 1rem 0;
  }
  
  .success {
    background: #f0fff4;
    border: 2px solid #68d391;
    color: #22543d;
    padding: 1rem;
    border-radius: 8px;
    margin: 1rem 0;
  }
  
  .warning {
    background: #fffbf0;
    border: 2px solid #f6ad55;
    color: #744210;
    padding: 1rem;
    border-radius: 8px;
    margin: 1rem 0;
  }
  
  .error {
    background: #fff5f5;
    border: 2px solid #fc8181;
    color: #742a2a;
    padding: 1rem;
    border-radius: 8px;
    margin: 1rem 0;
  }
  
  .columns {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 2rem;
    margin: 1.5rem 0;
  }
  
  .footer {
    position: absolute;
    bottom: 20px;
    right: 20px;
    font-size: 0.8rem;
    color: #718096;
  }

---

# 🚀 Roteiro de Codificação ASP.NET Core

## Tutorial Completo: Do Zero ao Deploy

*Guia passo a passo para desenvolvimento profissional*

---

## 📋 Pré-requisitos

### Ferramentas Necessárias
- ✅ .NET 8 SDK
- ✅ Visual Studio Code
- ✅ Git
- ✅ SQL Server LocalDB

### Conhecimentos
- 🎯 C# básico
- 🎯 Conceitos de APIs REST
- 🎯 Fundamentos de banco de dados

---

## <span class="step-number">1</span> Configuração do Ambiente

### Verificando Instalações

```bash
# Verificar .NET SDK
dotnet --version

# Verificar Git
git --version

# Listar SDKs instalados
dotnet --list-sdks
```

<div class="highlight">
💡 **Dica**: Certifique-se de ter o .NET 8 LTS instalado para melhor compatibilidade.
</div>

---

## <span class="step-number">2</span> Criando o Projeto

### Comando de Criação

```bash
# Criar novo projeto Web API
dotnet new webapi -n ProductAPI --use-controllers

# Navegar para o diretório
cd ProductAPI

# Adicionar pacotes necessários
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet add package Microsoft.EntityFrameworkCore.Design
```

### Estrutura Inicial

```
ProductAPI/
├── Controllers/
├── Models/
├── Data/
├── Services/
├── Program.cs
└── appsettings.json
```

---

## <span class="step-number">3</span> Definindo os Modelos

### Product.cs

```csharp
using System.ComponentModel.DataAnnotations;

namespace ProductAPI.Models
{
    public class Product
    {
        public int Id { get; set; }
        
        [Required(ErrorMessage = "Nome é obrigatório")]
        [StringLength(100, MinimumLength = 2)]
        public string Name { get; set; } = string.Empty;
        
        [StringLength(500)]
        public string Description { get; set; } = string.Empty;
        
        [Range(0.01, double.MaxValue, ErrorMessage = "Preço deve ser maior que zero")]
        public decimal Price { get; set; }
        
        public int Stock { get; set; }
        
        public bool IsActive { get; set; } = true;
        
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
        
        // Navigation property
        public int CategoryId { get; set; }
        public Category Category { get; set; } = null!;
    }
}
```

---

## <span class="step-number">4</span> Configurando Entity Framework

### ApplicationDbContext.cs

```csharp
using Microsoft.EntityFrameworkCore;
using ProductAPI.Models;

namespace ProductAPI.Data
{
    public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
        
        public DbSet<Product> Products { get; set; }
        public DbSet<Category> Categories { get; set; }
        
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            // Configurações de precisão para decimal
            modelBuilder.Entity<Product>(entity =>
            {
                entity.Property(e => e.Price)
                      .HasPrecision(18, 2);
                      
                entity.HasIndex(e => e.Name)
                      .IsUnique();
                      
                entity.HasOne(p => p.Category)
                      .WithMany(c => c.Products)
                      .HasForeignKey(p => p.CategoryId);
            });
            
            base.OnModelCreating(modelBuilder);
        }
    }
}
```

---

## <span class="step-number">5</span> Configurando Serviços (Program.cs)

```csharp
using Microsoft.EntityFrameworkCore;
using ProductAPI.Data;
using ProductAPI.Services;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container
builder.Services.AddControllers();

// Entity Framework
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

// Services
builder.Services.AddScoped<IProductService, ProductService>();

// Swagger
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// Configure the HTTP request pipeline
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();

app.Run();
```

---

## <span class="step-number">6</span> Implementando o Controller

### ProductsController.cs

```csharp
using Microsoft.AspNetCore.Mvc;
using ProductAPI.Models;
using ProductAPI.Services;

namespace ProductAPI.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
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
        
        [HttpGet]
        public async Task<ActionResult<IEnumerable<Product>>> GetProducts()
        {
            try
            {
                var products = await _productService.GetAllProductsAsync();
                return Ok(products);
            }
            catch (Exception ex)
            {
                _logger.LogError(ex, "Erro ao buscar produtos");
                return StatusCode(500, "Erro interno do servidor");
            }
        }
        
        [HttpGet("{id}")]
        public async Task<ActionResult<Product>> GetProduct(int id)
        {
            var product = await _productService.GetProductByIdAsync(id);
            
            if (product == null)
            {
                return NotFound($"Produto com ID {id} não encontrado");
            }
            
            return Ok(product);
        }
        
        [HttpPost]
        public async Task<ActionResult<Product>> CreateProduct(Product product)
        {
            if (!ModelState.IsValid)
            {
                return BadRequest(ModelState);
            }
            
            var createdProduct = await _productService.CreateProductAsync(product);
            
            return CreatedAtAction(nameof(GetProduct), 
                                 new { id = createdProduct.Id }, 
                                 createdProduct);
        }
    }
}
```

---

## <span class="step-number">7</span> Testando a API

### Usando Swagger

1. Execute a aplicação:
```bash
dotnet run
```

2. Acesse: `https://localhost:7xxx/swagger`

<div class="success">
✅ **Sucesso**: Se tudo estiver correto, você verá a interface do Swagger com seus endpoints listados.
</div>

### Testando com cURL

```bash
# GET - Listar produtos
curl -X GET "https://localhost:7xxx/api/products" -H "accept: application/json"

# POST - Criar produto
curl -X POST "https://localhost:7xxx/api/products" \
     -H "accept: application/json" \
     -H "Content-Type: application/json" \
     -d '{
       "name": "Produto Teste",
       "description": "Descrição do produto",
       "price": 29.99,
       "stock": 100,
       "categoryId": 1
     }'
```

---

## <span class="step-number">8</span> Próximos Passos

<div class="columns">
<div>

### 🔄 Melhorias Imediatas
- Implementar paginação
- Adicionar filtros de busca
- Configurar CORS
- Implementar cache

### 🔒 Segurança
- Autenticação JWT
- Rate limiting
- Validação de entrada
- HTTPS obrigatório

</div>
<div>

### 📊 Monitoramento
- Application Insights
- Health checks
- Logging estruturado
- Métricas customizadas

### 🚀 Deploy
- Docker containerization
- Azure App Service
- CI/CD Pipeline
- Monitoramento produção

</div>
</div>

---

## ⚠️ Problemas Comuns

<div class="warning">
**Erro de Connection String**: Verifique se a string de conexão no `appsettings.json` está correta.
</div>

<div class="error">
**Erro 500**: Geralmente indica problema de configuração do Entity Framework ou banco de dados.
</div>

<div class="highlight">
**Dica de Debug**: Use o logger para identificar onde o erro está ocorrendo.
</div>

---

## 📚 Recursos Adicionais

| Recurso | Link | Descrição |
|---------|------|-----------|
| Documentação | [docs.microsoft.com](https://docs.microsoft.com/aspnet/core) | Documentação oficial |
| Tutoriais | [Microsoft Learn](https://learn.microsoft.com) | Tutoriais interativos |
| Comunidade | [Stack Overflow](https://stackoverflow.com/questions/tagged/asp.net-core) | Q&A da comunidade |

---

## ✅ Checklist Final

- [ ] Projeto criado e executando
- [ ] Models definidos corretamente
- [ ] Entity Framework configurado
- [ ] Controller implementado
- [ ] Testes básicos realizados
- [ ] Swagger funcionando
- [ ] Logging configurado

<div class="footer">
Criado com ❤️ para desenvolvedores .NET
</div>