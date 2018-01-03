## Completar una entrada con un encasillado

Añadir entradas al to-do es genial, pero eventualmente también vas a tener que completar las tareas. En la vista `Views/Todo/Index.cshtml`, se pinta un encasillado para cada entrada del to-do.

```html
<input type="checkbox" name="@item.Id" value="true" class="done-checkbox">
```

El ID (guid) de la entrada es grabado en el atributo `name` del elemento. Puedes utilizar el ID para decirle al código de ASP.NET Core que actualice la entrada en la base de datos cuando el encasillado es seleccionado.

Así es como el flujo se vería:

* El usuario hace una marca en el encasillado, lo cual dispara una función en Javascript
* Javascript es usado para hacer la llamada API a una acción en el controlador
* La acción llama a la capa del servicio para hacer la actualización del elemento en la base de datos
* Una respuesta es enviada de regreso a la función de Javascript indicando que la acualización fue exitosa
* La página HTML es actualizada

### Add JavaScript code

Primero, abre el archivo `site.js` y añade este código al bloque de `$(document).ready`: 

**wwwroot/js/site.js**

```javascript
$(document).ready(function() {

    // ...

    // Wire up all of the checkboxes to run markCompleted()
    $('.done-checkbox').on('click', function(e) {
        markCompleted(e.target);
    });

});
```

Luego añade la función `markCompleted` al final del archivo:

```javascript
function markCompleted(checkbox) {
    checkbox.disabled = true;

    $.post('/Todo/MarkDone', { id: checkbox.name }, function() {
        var row = checkbox.parentElement.parentElement;
        $(row).addClass('done');
    });
}
```

Este código utiliza jQuery para enviar un HTTP POST a `http://localhost:5000/Todo/MarkDone`. Incluyendo en la llamada un parametro, `id`, el cual contiene el ID del elemento (obtenido del atributo `name`).

Is abres el Network Tools del web browser y seleccionas el encasillado, vas a poder ver la llamada así:

```
POST http://localhost:5000/Todo/MarkDone
Content-Type: application/x-www-form-urlencoded

id=<some guid>
```

La función que maneja el éxito pasada al `$.post` utiliza jQuery para añadir una clase a la fila de la tabla donde se encuentra el encasillado. Con la fila marcada con la clase `done`, una regla CSS en el stylesheel hace que cambie la manera que se ve la fila.

### Añadir la acción al controlador

Como te deber imaginar, tambien necesitas añadir la acción `MarkDone` al controlador `TodoController`:

```csharp
public async Task<IActionResult> MarkDone(Guid id)
{
    if (id == Guid.Empty) return BadRequest();

    var successful = await _todoItemService.MarkDoneAsync(id);

    if (!successful) return BadRequest();

    return Ok();
}
```

Veamos cada parte de este método. Primero, en su definición el método acepta un parametro `Guid` llamado `id`. Diferente a la acción `AddItem`, la cual utiliza un modelo (el modelo `NewTodoItem`) y model binding/validation, el parámetro `id` es bien simple. Si la llamada incluye el parametro llamado `id`, el ASP.NET Core lo tratará de interpretar como un guid.

No hay tal cosa como `ModelState` para validación, pero aún puedes verificar si el guid es válido. Si por alguna razón el parámetro `id` de la llamada faltase no podría ser interpretado como un guid, lo cual produciria un valor de `Guid.Empty`. Si ese fuese el caso, la llamada retornaría al comienzo:

```csharp
if (id == Guid.Empty) return BadRequest();
```

El método `BadRequest()` es un método de ayuda el cual simplemente devuelve el estatus HTTP de `400 Bad Reuest`.

Luego el controlador necesita hacer una llamada al servicio para actualizar la base de datos. Esto es manejado por un nuevo método llamada `MarkDoneAsync` en `ITodoItemService`, el cual devueleve cierto o falso dependiendo si la actualización fué exitosa:

```csharp
var successful = await _todoItemService.MarkDoneAsync(id);
if (!successful) return BadRequest();
```

Finalmente si todo se ve bien, el método `Ok()` es utilizado para devolver el estatus `200 OK`. APIs mas complejos pudiesen devolver un JSON o alguna otra data, por ahora devolver el estatus es todo lo que necesitas.

### Añadir un método de servicio

Primero, añadir `MarkDoneAsync` a la definicion de la interface:

**`Services/ITodoItemService.cs`**

```csharp
Task<bool> MarkDoneAsync(Guid id);
```

Luego, crear la implementación concreta en `TodoItemService`:

**`Services/TodoItemService.cs`**

```csharp
public async Task<bool> MarkDoneAsync(Guid id)
{
    var item = await _context.Items
        .Where(x => x.Id == id)
        .SingleOrDefaultAsync();

    if (item == null) return false;

    item.IsDone = true;

    var saveResult = await _context.SaveChangesAsync();
    return saveResult == 1; // One entity should have been updated
}
```

Este método utiliza el Entity Framework Core y `Where` para encontrar la entidad por el ID en la base de datos. El método `SingleOrDefaultAsync` devuelve el elemeto (si existe) o `null` si el ID es inválido. Si el ID no existe el código puede terminar rápido.

Una vez estás seguro que `item` no es nulo, es simplemente un a cosa de marcar la propiedad `IsDone`:

```csharp
item.IsDone = true;
```

Cambiar la propiedad solo afecta la copia local del elemento hasta que `SaveChangesAsync` es llamado para hacer que los cambios persistan en la base de datos. `SaveCHangesAsync` devuleve un entero el cual refleja cuantas entidades fueron actualizadas en la operación de grabar. En este caso, sería 1 (si se actualiza el elemento) o 0 (si algo salió mal).

### Pruébalo

Ejecuta la aplicación y marca algunos elementos en la lista. Refresca la página y van a desaparecer por completo, debido al filtro `Where` en el método `GetInCompleteItemAsync`.

Por ahora la aplicación contiene una sola lista compartida de to-do. Esto sería aún mas útil si mantuviese un registro de listas to-do individuales para cada usuario. En el próximo capítulo, utilizarás ASP.NET Core Identity para añadir seguridad y autenticación al proyecto.
