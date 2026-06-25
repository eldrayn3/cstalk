WebApiSolution
│
├── Domain
│   ├── Models
│   │   ├── Product.cs
│   │   └── Supplier.cs
│
├── Data
│   └── AppDbContext.cs
│
├── Services
│   └── InventoryService.cs
│
└── WebApiApp
    ├── Controllers
    │   └── InventoryController.cs
    ├── Program.cs
    └── appsettings.json

Product.cs

namespace Domain.Models;

public class Product
{
    public int Id { get; set; }

    public string Name { get; set; } = string.Empty;

    public decimal Price { get; set; }

    public int Stock { get; set; }

    public int SupplierId { get; set; }

    public Supplier? Supplier { get; set; }
}

Supplier.cs

namespace Domain.Models;

public class Supplier
{
    public int Id { get; set; }

    public string Name { get; set; } = string.Empty;

    public string Address { get; set; } = string.Empty;

    public ICollection<Product> Products { get; set; }
        = new List<Product>();
}

***AppDbContext.cs

using Domain.Models;
using Microsoft.EntityFrameworkCore;

namespace Data;

public class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions<AppDbContext> options)
        : base(options)
    {
    }

    public DbSet<Product> Products { get; set; }

    public DbSet<Supplier> Suppliers { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);

        modelBuilder.Entity<Supplier>().HasData(
            new Supplier
            {
                Id = 1,
                Name = "ABC Suppliers",
                Address = "New York"
            },
            new Supplier
            {
                Id = 2,
                Name = "Global Traders",
                Address = "Chicago"
            }
        );

        modelBuilder.Entity<Product>().HasData(
            new Product
            {
                Id = 1,
                Name = "Laptop",
                Price = 1200,
                Stock = 20,
                SupplierId = 1
            },
            new Product
            {
                Id = 2,
                Name = "Mouse",
                Price = 30,
                Stock = 100,
                SupplierId = 2
            }
        );
    }
}

***InventoryService.cs

using Data;
using Domain.Models;
using Microsoft.EntityFrameworkCore;

namespace Services;

public class InventoryService
{
    private readonly AppDbContext _context;

    public InventoryService(AppDbContext context)
    {
        _context = context;
    }

    public async Task<List<Product>> GetAllProductsAsync()
    {
        return await _context.Products
            .Include(p => p.Supplier)
            .ToListAsync();
    }

    public async Task<List<Product>> GetProductsAbovePriceAsync(decimal price)
    {
        return await _context.Products
            .Include(p => p.Supplier)
            .Where(p => p.Price > price)
            .ToListAsync();
    }

    public async Task<Product> AddProductAsync(Product product)
    {
        _context.Products.Add(product);
        await _context.SaveChangesAsync();
        return product;
    }

    public async Task<bool> DeleteProductAsync(int id)
    {
        var product = await _context.Products.FindAsync(id);

        if (product == null)
            return false;

        _context.Products.Remove(product);
        await _context.SaveChangesAsync();

        return true;
    }
}

***Controllers/InventoryController.cs

using Data;
using Domain.Models;
using Microsoft.EntityFrameworkCore;

namespace Services
{
    public class InventoryService
    {
        private readonly AppDbContext _context;

        public InventoryService(AppDbContext context)
        {
            _context = context;
        }

        // Get all products including their suppliers
        public async Task<List<Product>> GetAllProductsAsync()
        {
            return await _context.Products
                .Include(p => p.Supplier)
                .ToListAsync();
        }

        // Get products whose price exceeds a threshold
        public async Task<List<Product>> GetProductsAbovePriceAsync(decimal threshold)
        {
            return await _context.Products
                .Include(p => p.Supplier)
                .Where(p => p.Price > threshold)
                .ToListAsync();
        }

        // Add a new product
        public async Task<Product> AddProductAsync(Product product)
        {
            _context.Products.Add(product);
            await _context.SaveChangesAsync();

            return product;
        }

        // Delete a product by ID
        public async Task<bool> DeleteProductAsync(int id)
        {
            var product = await _context.Products.FindAsync(id);

            if (product == null)
            {
                return false;
            }

            _context.Products.Remove(product);
            await _context.SaveChangesAsync();

            return true;
        }
    }
}

***Program.cs

using Data;
using Microsoft.EntityFrameworkCore;
using Services;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();

builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

builder.Services.AddDbContext<AppDbContext>(options =>
{
    options.UseInMemoryDatabase("InventoryDb");
});

builder.Services.AddScoped<InventoryService>();

var app = builder.Build();

if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();

app.MapControllers();

using (var scope = app.Services.CreateScope())
{
    var context =
        scope.ServiceProvider.GetRequiredService<AppDbContext>();

    context.Database.EnsureCreated();
}

app.Run();
