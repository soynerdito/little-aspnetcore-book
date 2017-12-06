# Añadir paquetes externos
Una de las grandes ventajas de usar un ambiente maduro como .NET es el ecosistema de paquetes de terceros y plugins es grande. Asi como lo es en otros repositorios de paquetes (npm, Maven, RubyGems), se pueden bajar he instalar paquetes de .NET para ayudar en practicamente cualquier tarea o problema que uno se pueda imaginar. 

Nuget es la herramienta para instalar paquetes asi como el repositorio de paquetes oficial (en https://www.nuget.org)). Puedes buscar por paquetes de Nuget en la web he instalarlos en tu maquina local atravez de la consola o terminal (o con u GUI cuando se usa Visual Studio).

## Instalar el paquete Humanizer
Al final del último capítulo, la aplicación to-do muestra el listado de to-do de la siguiente manera:

![Dates in ISO 8601 format](iso8601.png)

La fecha de vencimiento se muestra en un formato que es bueno para las máquinas (llamado ISO 8601), pero resulta complejo para los humanos. No seria mas agradable leer "X días desde hoy"? Puedes escribir el codigo que haga dicha conversion a un texto mas amigable para un humano, pero afortunadamente hay una manera mas sencilla.

El paquete Humanizer en NuGet (https://www.nuget.org/packages/Humanizer) resuelve este problema. Provee metodos que pueden "humanizar" o re-escribir casi cualquier cosa: fechas, horas, duraciones, numeros y similares. Es un proyecto de codigo abierto fantástico y muy útil publicado bajo la permisiva licensia MIT.

Para añadirlo a tu proyecto, ejecuta el siguiente comando en el terminal:

```
dotnet add package Humanizer
```

Si miras el archivo `AspNetCoreTodo.csproj`, puedes ver una nueva linea `PackageReference` la cual hace referencia a `Humanizer`.

## Usar Humanizer en una vista

Para usar el paquete en tu código, usualmente basta con añadir una línea `using` al principio del archivo para importar el paquete.

Ya que el Humanizer va a ser usado para pintar fechas en la vista, puede ser usado directamente en la vista. Primero, añades la instrucción de `@using` al principio de la vista:

**`Views/Todo/Index.cshtml`**

```html
@model TodoViewModel;
@using Humanizer;

// ...
```

Luego, actualiza la linea que escribe la propiedad `DueAt` para usar el método de Huminizar `Humanize`:

```html
<td>@item.DueAt.Humanize()</td>
```

Ahora las fechas son mucho mas legibles:

![Human-readable dates](friendly-dates.png)

Hay paquetes disponibles en NuGet para todo. Desde convertir XML a aprendizaje automático para ser compartirlo en Twitter. El mismo ASP.NET Core, bajo las sábanas, no es mas que una colección de paquetes de NuGet que son añadidos a tu proyecto.

> El archivo de poryecto creado por `dotnet new mvc` incluye una sola referencia al paquete `Microsoft.AspNetCore.All`, el cual es convenientemente el "super paquete" que referencia todos los demás paquetes necesarios tipicamente para un proyecto de ASP.NET Core. D esta manera no hace falta referenciar cientos de paquetes en el archivo que define el project.

En el siguiente capítulo, podrás ver otra serie de paquetes de NuGet (un sistema conocido como Entity Framework Core) el cual permite escribir códico que interactua con una base de datos.
