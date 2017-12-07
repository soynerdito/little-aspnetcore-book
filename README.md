# Introducción

Gracias por tomar el libro Little ASP.NET Core! Escribí este pequeño libro para ayudar a los desarolladores y a personas interesadas en aprender a programar para el web utilizando ASP.NET Core 2.0, una nueva plataforma para contruir aplicaciones y APIs.

El libro The Little ASP.NET Core Book está estructurado en forma de tutorial. Podrás contruir una aplicacion de to-do de principio a fin y aprenderás:

* Lo básico del patron MVC \(Modelo-Vista-Controlador\)
* Como el codigo de en frente \(HTML, CSS, Javascript\) trabaja en conjunto con el codigo de atrás
* Que es injección de dependencia y como es útil
* Como leer y escribir a una base de datos.
* Como añadir acceso, registro y seguridad
* Como publicar una aplicación al web

No te preocupes, no tienes que saber nada sobre ASP.NET Core \(o algo de lo arriba mencionado\) para comenzar.

## Antes de comenzar

El código de la aplicación que irás construyendo lo puedes encontrar completado en GitHub \([https://www.github.com/nbarbettini/little-aspnetcore-todo](https://www.github.com/nbarbettini/little-aspnetcore-todo)\). Sientete en la libertad de bajarlo si deseas y compararlo con el código tuyo.

Este libro se le hacen actualizaciones frecuentemente con arreglos de bugs y nuevo contenido. SI estar leyendo un PDF, e-book o la versión impresa, verifica la página oficial \([littleasp.net/book](http://www.littleasp.net/book)\) para verificar si existe una versión mas actualizada. En la página final contiene el la información sobre la version y el registro de cambios.

### Leyendo en tu mismo idioma

Gracias a unos amigos fantásticos multi-lenguaje, el libro Little ASP.NET Core ha sido traducido a otros lenguajes:

* **ASP.NET Core El Kitabı** \(Turkish\)

  [https://sahinyanlik.gitbooks.io/kisa-asp-net-core-kitabi/](https://sahinyanlik.gitbooks.io/kisa-asp-net-core-kitabi/)

* **简明 ASP.NET Core 手册** \(Simplified Chinese\)

  [https://windsting.github.io/little-aspnetcore-book/book/](https://windsting.github.io/little-aspnetcore-book/book/)

## Para quien es este libro

Si eres un programador nuevo, este libro te introduce los patrones y conceptos que se utilizan para construir aplicaciones web modernas. Vas a aprender como construir una aplicación web \(y como se unen las piezas grandes\) construyendo algo desde cero! Mientras este pequeño libro no puede cubrir absolutamente todo lo que necesitas saber sobre progrmación, te da un punto de partida para que puedas aprender temas más avanzados.

Si ya haces código en un lenguaje como Node, Python, Ruby, Go o Java vas a encotrar muchas ideas familiares como MVC, plantillas de vistas y injección de dependencia. El código va a ser en C\#, pero no se verá muy diferente a lo que ya conoces.

Si eres un desarrollador de ASP.NET MVC, te sentirás como en casa! ASP.NET Core añade unas nuevas herramientas y reusa \(y simplifica\) las cosas que ya conoces. Yo señalare algunas de estas diferencias luego.

No importa cual sea tu experiencia previo con programación web, este libro busca enseñarte todo lo que necesitas para crear una aplicacion web simple y útil en ASP.NET Core. Aprenderás como contruir funcionalidad en el codigo de atras asi como en el de delante. Como interactuar con una base de datos y como probar y publicar la aplicación al mundo.

## ¿Que es ASP.NET Core?

ASP.NET Core es una plataforma creada por Microsoft para hacer aplicaciones web, APIs y micro-servicios. Utiliza un patrón conocido como lo es MVC \(Modelo-Vista-Controlador\), inyección de dependencia y un manejo de llamadas compuesta de componentes conocidos como "middleware". Es de código abierto bajo la lisencia de Apache 2.0, lo que significa que el código fuente esta disponible libre y se fomenta a la comunidad a contribuir con arreglos y mejoras.

ASP.NET Core corre sobre el motor de Microsoft .NET similar a Java Virtual Machine \(JVM\) o el interpretador de Ruby. Puedes escribir tu aplicación ASP.NET Core en varios lenguajes \(C\#, Visual Basic, F\#\). C\# es el más popular y es ql eu usaré en este libro. Puedes contruir y ejectuar aplicaciones en ASP.NET COre tanto en WIndows, Mac como Linux.

## Why do we need another web framework?

There are a lot of great web frameworks to choose from already: Node/Express, Spring, Ruby on Rails, Django, Laravel, and many more. What advantages does ASP.NET Core have?

* **Speed.** ASP.NET Core is fast. Because .NET code is compiled, it executes much faster than code in interpreted languages like JavaScript or Ruby. ASP.NET Core is also optimized for multithreading and asynchronous tasks. It's common to see a 5-10x speed improvement over code written in Node.js.

* **Ecosystem.** ASP.NET Core may be new, but .NET has been around for a long time. There are thousands of packages available on NuGet \(the .NET package manager; think npm, Ruby gems, or Maven\). There are already packages available for JSON deserialization, database connectors, PDF generation, or almost anything else you can think of.

* **Security.** The team at Microsoft takes security seriously, and ASP.NET Core is built to be secure from the ground up. It handles things like sanitizing input data and preventing cross-site request forgery \(XSRF\) automatically, so you don't have to. You also get the benefit of static typing with the .NET compiler, which is like having a very paranoid linter turned on at all times. This makes it harder to do something you didn't intend with a variable or chunk of data.

## .NET Core and .NET Standard

Throughout this book, you'll be learning about ASP.NET Core \(the web framework\). I'll occasionally mention the .NET runtime \(the supporting library that runs .NET code\).

You may also hear about .NET Core and .NET Standard. The naming gets confusing, so here's a simple explanation:

**.NET Standard** is a platform-agnostic interface that defines what features and APIs are available in .NET. .NET Standard doesn't represent any actual code or functionality, just the API definition. There are different "versions" or levels of .NET Standard that reflect how many APIs are available \(or how wide the API surface area is\). For example, .NET Standard 2.0 has more APIs available than .NET Standard 1.5, which has more APIs than .NET Standard 1.0.

**.NET Core** is the .NET runtime that can be installed on Windows, Mac, or Linux. It implements the APIs defined in the .NET Standard interface with the appropriate platform-specific code on each operating system. This is what you'll install on your own machine to build and run ASP.NET Core applications.

And just for good measure, **.NET Framework** is a different implementation of .NET Standard that is Windows-only. This was the only .NET runtime until .NET Core came along and opened .NET up to Mac and Linux. ASP.NET Core can also run on Windows-only .NET Framework, but I won't touch on this too much.

If you're confused by all this naming, no worries! We'll get to some real code in a bit.

## A note to ASP.NET 4 developers

If you haven't used a previous version of ASP.NET, skip ahead to the next chapter!

ASP.NET Core is a complete ground-up rewrite of ASP.NET, with a focus on modernizing the framework and finally decoupling it from System.Web, IIS, and Windows. If you remember all the OWIN/Katana stuff from ASP.NET 4, you're already halfway there: the Katana project became ASP.NET 5 which was ultimately renamed to ASP.NET Core.

Because of the Katana legacy, the `Startup` class is front and center, and there's no more `Application_Start` or `Global.asax`. The entire pipeline is driven by middleware, and there's no longer a split between MVC and Web API: controllers can simply return views, status codes, or data. Dependency injection comes baked in, so you don't need to install and configure a container like StructureMap or Ninject if you don't want to. And the entire framework has been optimized for speed and runtime efficiency.

Alright, enough introduction. Let's dive in to ASP.NET Core!

