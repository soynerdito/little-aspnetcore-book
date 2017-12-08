# Introducción

Gracias por tomar El pequeño libro de ASP.NET Core! Escribí este pequeño libro para ayudar a los desarolladores y a personas interesadas en aprender a programar para el web utilizando ASP.NET Core 2.0, una nueva plataforma para contruir aplicaciones y APIs.

El libro The Little ASP.NET Core Book está estructurado en forma de tutorial. Podrás contruir una aplicacion de to-do de principio a fin y aprenderás:

* Lo básico del patron MVC (Modelo-Vista-Controlador)
* Como el codigo de en frente (HTML, CSS, Javascript) trabaja en conjunto con el codigo de atrás
* Que es injección de dependencia y como es útil
* Como leer y escribir a una base de datos.
* Como añadir acceso, registro y seguridad
* Como publicar una aplicación al web

No te preocupes, no tienes que saber nada sobre ASP.NET Core (o algo de lo arriba mencionado) para comenzar.

## Antes de comenzar

El código de la aplicación que irás construyendo lo puedes encontrar completado en GitHub ([https://www.github.com/nbarbettini/little-aspnetcore-todo](https://www.github.com/nbarbettini/little-aspnetcore-todo)). Sientete en la libertad de bajarlo si deseas y compararlo con el código tuyo.

Este libro se le hacen actualizaciones frecuentemente con arreglos de bugs y nuevo contenido. SI estar leyendo un PDF, e-book o la versión impresa, verifica la página oficial ([littleasp.net/book](http://www.littleasp.net/book)) para verificar si existe una versión mas actualizada. En la página final contiene el la información sobre la version y el registro de cambios.

### Leyendo en tu mismo idioma

Gracias a unos amigos fantásticos multi-lenguaje, el libro Little ASP.NET Core ha sido traducido a otros lenguajes:

* **ASP.NET Core El Kitabı** (Turkish)

  [https://sahinyanlik.gitbooks.io/kisa-asp-net-core-kitabi/](https://sahinyanlik.gitbooks.io/kisa-asp-net-core-kitabi/)

* **简明 ASP.NET Core 手册** (Simplified Chinese)

  [https://windsting.github.io/little-aspnetcore-book/book/](https://windsting.github.io/little-aspnetcore-book/book/)

## Para quien es este libro

Si eres un programador nuevo, este libro te introduce los patrones y conceptos que se utilizan para construir aplicaciones web modernas. Vas a aprender como construir una aplicación web (y como se unen las piezas grandes) construyendo algo desde cero! Mientras este pequeño libro no puede cubrir absolutamente todo lo que necesitas saber sobre progrmación, te da un punto de partida para que puedas aprender temas más avanzados.

Si ya haces código en un lenguaje como Node, Python, Ruby, Go o Java vas a encotrar muchas ideas familiares como MVC, plantillas de vistas y injección de dependencia. El código va a ser en C\#, pero no se verá muy diferente a lo que ya conoces.

Si eres un desarrollador de ASP.NET MVC, te sentirás como en casa! ASP.NET Core añade unas nuevas herramientas y reusa (y simplifica) las cosas que ya conoces. Yo señalare algunas de estas diferencias luego.

No importa cual sea tu experiencia previo con programación web, este libro busca enseñarte todo lo que necesitas para crear una aplicacion web simple y útil en ASP.NET Core. Aprenderás como contruir funcionalidad en el codigo de atras asi como en el de delante. Como interactuar con una base de datos y como probar y publicar la aplicación al mundo.

## ¿Que es ASP.NET Core?

ASP.NET Core es una plataforma creada por Microsoft para hacer aplicaciones web, APIs y micro-servicios. Utiliza un patrón conocido como lo es MVC (Modelo-Vista-Controlador), inyección de dependencia y un manejo de llamadas compuesta de componentes conocidos como "middleware". Es de código abierto bajo la lisencia de Apache 2.0, lo que significa que el código fuente esta disponible libre y se fomenta a la comunidad a contribuir con arreglos y mejoras.

ASP.NET Core corre sobre el motor de Microsoft .NET similar a Java Virtual Machine (JVM) o el interpretador de Ruby. Puedes escribir tu aplicación ASP.NET Core en varios lenguajes (C#, Visual Basic, F#). C# es el más popular y es ql eu usaré en este libro. Puedes contruir y ejectuar aplicaciones en ASP.NET COre tanto en WIndows, Mac como Linux.

## ¿Por que necesitamos otra plataforma web?

Existen muchas plataformas web bunas para escoger como: Node/Express, Spring, Ruby en Rails, Django, Laravel y muchas otras. ¿Que ventaja tenemos al escoger ASP.NET Core?

* **Velocidad.** ASP.NET Core es rápidos ya que .NET es compilado. Este se ejecuta mucho más rápido que el codigo interpretado como JavaScript o Ruby. ASP.NET Core está optimizado para ser multi proceso y con tareas asincronas. Es común ver una mejoría en velocidad de un 5-10 veces sobre código escrito en Node.js.

* **Ecosistema. **ASP.NET Core puede ser nuevo pero .NET lleva mucho tiempo. Hay miles de paquetes disponibles através de NuGet (la biblioteca de paquetes de .NET; piensa en npm, Ruby gems o Maven). Hay paquetes disponibles para dese-serialización JSON, conectores a bases de datos, generación de PDF o cualquier otra cosa que pudieras imaginar.

* **Seguridad.** El équipo de Microsoft se toma la seguridad muy en serio y ASP.NET Core está contruido para ser seguro desde sus simientos. Maneja cosas tales como limpieza de la data de entrada asi como previene "cross-site request forgery" (XSRF) automaticamente para que no tengas que hacerlo tú. Además optienes el beneficio de escrito estático con el compilador .NET, el cual es como tener un verificador paranoico todo el tiempo. Esto hace más dificil que hagas algo no deseado con un grupo de datos.

## .NET Core y Estándar .NET

A través de este libro vas apreder sobre ASP.NET Core (la plataforma web). Ocacionalmente mencionaré el motor .NET (las librerias de soporte que ejecutan el código .NET).

También puedes ademas escuchar cobre .NET Core y Estándar .NET. El nombre crea confusió, asi que aquí una pequeña explicación:

**Estándar .NET **es una interface de plataforma agnóstica que define las capacidades y APIs disponibles en .NET. El Estándard .NET no representa ningún código actual o funcionalidad, es solo la definicion de un API. La diferencia entre las distintas "versiones" o niveles del Estandard .NET refleja cuantos APIs están disponible (o cuan amplia es la covertura de API). Por ejemplo, el Estandard .NET 2.0 tiene mas APIs disponibles que el Estandard .NET 1.5 el cual a su vez tiene mas APIs que el Estándard .NET 1.0.

**.NET Core** es el motor .NET que es instalado en WIndows, Mac o Linux. Este implemeta los APIs definidos en el Estandard .NET con su respectivo código especifico para cada arquitectura y sistema operativo. Esto es lo que instalaras en tú maquina para contruir y ejectuar aplicaciones .NET Core.

Y por buena mesura, **.NET Framework**  es una implemetación diferente al Estandard .NET que es sólo para Windows. Este era el único motor .NET hasta que llego .NET Core y abrió .NET a Mac y Linux. ASP.NET Core puede tambié ejcutarse en .NET Framework de sólo Windows, pero no vamos a entrar mucho en esto.

Si estás confundido por todos estos nombres, no hay te preocupes. Vamos a entrar en código real muy pronto.

## Una nota para desarrolladores de ASP.NET 4

Si alguna vez has usado una versión previa de ASP.NET, salta ahora hasta el proximo capítulo.

ASP.NET Core es una nueva version de ASP.NET enfocado en modernizar  ls plataforma y finalmente separarlo de System.Web, IIS y Windows. Si recuerdas todo sobre el asunto entre ASP.NET 4 y OWIN/Katana ya estás a medio camino: el proyecto Katana se convirtió en ASP.NET 5 el cual finalmente se renombró a ASP.NET Core.

Por el legado de Katana, la clase `Startup` es el frente y centro y no hay mas `Application_Start` o `Global.asax`. El andiamaje completo es manejado por middleware y no hay mas una separacion entre MVC y Web API: los controladores pueden simplmente devolver vistas, códigos de estado o datos. Ijección de dependencia esta dado así que no tienes que configurar un contenedor como StructureMap o Ninject si no lo quieres. Y la plataforma completa ha sido optimizada para velocidady eficiencia.

De acuerdo, ya basta de introducción. Vamos a adentrarnos en ASP.NET Core.

