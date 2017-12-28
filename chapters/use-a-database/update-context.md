## Actualizar el context

No hay mucho en el context de la base de datos aún:

**`Data/ApplicationDbContext.cs`**

```csharp
public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder);
        // Customize the ASP.NET Identity model and override the defaults if needed.
        // For example, you can rename the ASP.NET Identity table names and more.
        // Add your customizations after calling base.OnModelCreating(builder);
    }
}
```

Añade una propiedad `DbSet` al `ApplicationDbContext`, justo bajo el constructor:

```csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}

public DbSet<TodoItem> Items { get; set; }

// ...
```

Un `DbSet` representa una tabla o lista en la base de datos. Al crear una propiedad `DbSet<TodoItem>` llamada `Items`, le decimos al Entity Framework Core que quieres almacenar entidades tipo `TodoItem` en una tabla llamada `Items`.

Con esto haz actualizado la clase context, pero hay un pequeño problema. El context y la base de datos no están sincronizados ya que no existe una tabla llamada `Items` en la base datos. (Solamente cambiar el código no hace cambios en la base da datos como tal.) 

Para que se reflejen los cambios hechos en el context con la base de datos, debes crear una **migration**.

> Si ya tienes una base de datos existente, búsca en la web por "scaffold-dbcontext existing database" y leé la documentacion de Microsoft en usando el `Scaffold-DbContext` para hacerle reverse-engineer a la estructura de la base de datos y llevarla a un `DbContext` y clases de moles automaticamente.
