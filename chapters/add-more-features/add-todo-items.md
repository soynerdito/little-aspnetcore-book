## Añadir una entrada nueva al to-do

El usuario añadirá una nueva entrada al to-do con una forma simple bajo el listado:

![Final form](final-form.png)

Añadir esta funcionalidad requiere los siguientes pasos:

* Añadir un JavaScript que envie la información al servidor
* La creación de una acción en el controlador que maneje esta solicitud
* Añadir códico a la capa de servicio que actualice la base de datos

### Add JavaScript code

La vista `Todo/Index.cshtml` ya incluye una forma en HTML la cual contiene un campo de texto y un boton para añadir una entrada nueva. Usarás jQuery para enviar un POST al servidor cuando se oprima el botón de Add.

Abre el archivo `wwwroot/js/site.js` y añade el siguiente código:

```javascript
$(document).ready(function() {

    // Wire up the Add button to send the new item to the server
    $('#add-item-button').on('click', addItem);

});
```

Luego escribes la función de `addItem` al final del archivo:

```javascript
function addItem() {
    $('#add-item-error').hide();
    var newTitle = $('#add-item-title').val();

    $.post('/Todo/AddItem', { title: newTitle }, function() {
        window.location = '/Todo';
    })
    .fail(function(data) {
        if (data && data.responseJSON) {
            var firstError = data.responseJSON[Object.keys(data.responseJSON)[0]];
            $('#add-item-error').text(firstError);
            $('#add-item-error').show();
        }
    });
}
```

Esta funcion va a enviar un POST a `http://localhost:5000/Todo/AddItem` con el nombre del usuario escrito. El tercer parametro pasado al método de `$.post` (la funcion) es el manejador de resultado correcto el cual va a ser ejecutado si el servidor responde con `200 OK`. La funcion de manejo de éxito utiliza `window.location` para refrescar la página (mediante posicionando la dirección a `/Todo`, la misma página que esta activada actualmente). Si el servidor responde con `400 Bad Request`, el manejador `fail` incluido al método `$.post` va a tratar de extrar un mesaje de error y mostrarlo en un `<div>` con indetificado `add-item-error`.

### Añadir una acción

El código JavaScript anterior aún no funciona, ya que no existe una accion que pueda manejar la ruta `/Todo/AddItem`. Si lo intentas ahora, ASP.NET Core devolveria un error `404 Not Found`.

Debes crear una nueva acción llamada `AddItem` al controlador `TodoController`:

```csharp
public async Task<IActionResult> AddItem(NewTodoItem newItem)
{
    if (!ModelState.IsValid)
    {
        return BadRequest(ModelState);
    }

    var successful = await _todoItemService.AddItemAsync(newItem);
    if (!successful)
    {
        return BadRequest(new { error = "Could not add item" });
    }

    return Ok();
}
```

En la llamada de este método se define un parametro `NewTodoItem`, el cual es un model que no existe aún.
Es necesario crearlo:

**`Models/NewTodoItem.cs`**

```csharp
using System;
using System.ComponentModel.DataAnnotations;

namespace AspNetCoreTodo.Models
{
    public class NewTodoItem
    {
        [Required]
        public string Title { get; set; }
    }
}
```

La definición de este modelo (una propiedad llamada `Title`) concuerda con la data que se envia en la accion del jQuery:

```javascript
$.post('/Todo/AddItem', { title: newTitle }  // ...

// A JSON object with one property:
// {
//   title: (whatever the user typed)
// }
```

ASP.NET Core utiliza un proceso llamado **model binding** para cazar los parametros sometidos en el POST con la definición del modelo creado. Si los nombres de los parametros son los mismos (ignorando cosas como mayúsculas o mínusculas) la data enviada va a ser copiada en el modelo.

Luego de cazar la data enviada al modelo, ASP.NET Core ejecuta la **validación del modelo**. El atributo `[Required]` en la propiedad `Title` le informa al validador que la propiedad `Title` no puede estar vacia o en blanco. El validador no tira un error si el modelo falla, pero el estado de validación se guarda para que pueda ser verificado en el controlador.

> Sidebar: Es posible reusar el modelo `TodoItem` en lugar de crear un modelo `NewTodoItem`, pero `TodoItem` contiene propiedades que nunca serán sometidas por el usario (ID y done). Es mas limpio declarar un modelo nuevo que represente exatamente las propiedades relevantes al añadir una entrada nueva.

De vuelta al método `AddItem` en el controlador `TodoController`: el primer bloque verifica si el modelo aprovó o no el proceso de validacion. Es costumbre hacer esta verificacion justo al principio del método:

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

Si el estado del modelo `ModelState` es inválido (por la propiedad requerida está en blanco), la acción devolveria un 400 Bad Request asi como el estado del modelo, el cual automaticamente se convierte en un mensaje de error que le dice al usuario que está mal.

Luego, el controlador llama a la capa de servicio para hacer la operacion contra la base de datos:

```csharp
var successful = await _todoItemService.AddItemAsync(newItem);
if (!successful)
{
    return BadRequest(new { error = "Could not add item." });
}
```

El método `AddItemAsync` devuelve `true` (cierto) o `false` (falso) dependiendo si la entrada fue satisfactoriamente añadida a la base de datos. Si por alguna razón falla, esta acción devuelve `400 Bad Request` (solicitud equivocada) asi com un objeto que contiene la propiedad `error`.

Finalmente, si todo es completado sin errores, la acción que se devuelve es `200 OK`.

### Añadir un método de servicio

Si estas usando un editor de texto que entienda C#, podrás ver unas rallitas rojas bajo `AddItemAsync` ya que este método aún no existe.

Como un último paso, debes añadir un método a la capa de servicio. Primero, añadirlo a la definición de la interface `ITodoItemService`:

```csharp
public interface ITodoItemService
{
    Task<IEnumerable<TodoItem>> GetIncompleteItemsAsync();

    Task<bool> AddItemAsync(NewTodoItem newItem);
}
```

Luego a su implementacion en `TodoItemService`:

```csharp
public async Task<bool> AddItemAsync(NewTodoItem newItem)
{
    var entity = new TodoItem
    {
        Id = Guid.NewGuid(),
        IsDone = false,
        Title = newItem.Title,
        DueAt = DateTimeOffset.Now.AddDays(3)
    };

    _context.Items.Add(entity);

    var saveResult = await _context.SaveChangesAsync();
    return saveResult == 1;
}
```

Este método crea un nuevo `TodoItem` (el modelo que represneta la entidad de la base de datos) y copia el `Title` del modelo `NewTodoItem`. Entonces este se añade al contexto y se usa `SaveChangesAsync` para persistir la entidad en la base de datos.

> Sidebar: Este emplo es una manera de añadir funcionalidad. Si deseas mostrar una pagina nueva para añadir una entrada (para una entidad complicada con muchas propiedades, por ejemplo), puedes crear una nueva vista que esté atada al modelo al cual el usario le proveera valores. ASP.NET Core puede pintar la forma automaticamente con las propiedades del modelo utilizando algo llamado **tag helpers**. Puedes buscar ejmplos en la documentacion de ASP.NET Core en https://docs.asp.net.

### Pruebalo

Ejecuta la aplicacion y añade varias entradas al listado de to-do con la forma. Ya que las entradas están siendo guardadas en la base de datos. Estos van a estar alli luego que la aplicación se detenga y se eche a correr nuevamente.

> Como un reto futuro, trata de añadir un selector de fecha usando HTML y JavaScript, que permita al usuario seleccionar (opcional) una fecha para la propiedad de  `DueAt`. Entonces utiliza esa fecha en vez de crear automaticamente las tareas con una fecha de vencimiento de 3 dias.
