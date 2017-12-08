## Requerir autenticación

Comunmente quieres requerir que el usuario haga log in antes de tener acceso a diferentes areas de la aplicació. Por ejemplo hace sentido mostrar la página principal a todos \(independientemente que haya iniciado una sessión o no\) pero solo mostrar la lista de to-do luego de inciar una sessión.

Puede utilizar el atributothe `[Authorize]`  en ASP.NET Core para requerir que el usuario haya sido validado para alguna accion en particular o para todo el controlador. Para requerir autenticacion por todo el `TodoController`, añade el atributo sobre la primera linea del controlador:

```csharp
[Authorize]
public class TodoController : Controller
{
    // ...
}
```

Añade esta linea `using`en el tope del archivo:

```csharp
using Microsoft.AspNetCore.Authorization;
```

Intenta ejecutar la aplicació y acceder `/todo` sin antes hacer log in. Vas a ser redirijido a la página de login automaticamente.

> A pesar del nombre del atributo, nosotros estamos realmente una verificación de autenticación y no autorización. Disculpa la confusión.



