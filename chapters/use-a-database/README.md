# Utilizar una base de datos

Escribir código de base de datos puede ser difícil. A menos que realmente sepas lo que estás haciendo, es una mala idea pegar queries SQL directamente a en el código de la aplicación. Un **object-relational mapper** (ORM) hace que escribir código que interactua con una base de datos sea más fácil añadiendo una capa de abstracción entre su código y la base de datos como tal. En Java Hibernate y en Rubi ActiveRecord son dos de los ORMs más conocidos.

Hay varios ORMs para .NET, incluyendo en que está integrado por Microsoft e incluido en ASP.NET Core por defecto: Entity Framework Core. El Entity Framework Core hace que sea fácil conectarse a un diferentes tipos de bases de datos y te permite utilizar tú código de C# para crear queris de la base de datos sean transformados de vuelta a modelos C# (POCOs).

> ¿Recuerda como creando una interface de servicio separa el código del controlador a la clase concreata del servicio? El Entity Framework Core es como una interface bien grande sobre tu base de datos y te permite cambiar proveedores dependiendo de la tecnología de base de datos utilizada.

El Entity Framework Core puede conectarse a una base de datos como SQL Server y MySQL y además con funciona con bases de datos NoSQL (de documento) como Mongo. Puedes utilizar una base de datos SQLite para este proyecto, pero tambien puedes conectar proveedor de base de datos distinto si asi lo deseas.