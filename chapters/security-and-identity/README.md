# Seguridad he identidad

Seguridad es factor bien importante en cualquier aplicacion web o API moderno. Es importante mantener la data del usuario o cliente segura y lejos de las manos de un atacante. Esto está compuesto de lo siguiente

* Limpieza de la data de entrada para prevenir ataques de SQL injection
* Prevenir cross-domain (XSRF) ataques en formas
* Utilizar HTTPS (TLS) para que la data no pueda ser interceptada por el Internet
* Proveer al usuario una manera segura de acceder con una clavo o credenciales de redes sociales
* Diseñar una manera de restaurar la clave o autenticacion de multiple-factor con seguridad en mente

ASP.NET Core puede ayudar hacer la implementación de todo esto facil. Las primeras dos (protección a SQL injection y cross-domain tacks) ya están incluidas y solo hay que añadir unas pocosa lineas de codigo para activar soporte a HTTPS. Este capítulo esta enfocado mayormente en la seguridad de **identidad**: manejando cuentas de usuario (registracción, cuentas), authenticación (acceso) a tus usuarios de una manera segura tomando desiciones de autorizacion una vez el usuario ha sido autenticado.

> Autenticació y autorizacion son ideas diferentes y comúnmente son confundidades. **Autenticación** maneja cuando el usuario accede la aplicación mientras que **autorización **se encarga de lo que se le permite hacer al usuario una vez accede la aplicación. Puede visualizar autenticacion haciendo la pregunta "¿Se quier es este usuario?" Mientras que autorización pregunta "Acaso este usuario tiene permiso apra hacer X cosa?"

La plantilla MVC + Autenticación Individual incluida en el andamiaje que usaste para crear el proyecto contiene una serie de clases sobre ASP.NET Core Identity, un sistema de autenticación e identidad es parte del ASP.NET Core.

## Qué es ASP.NET Core Identity?

ASP.NET Core Identity es el sistema de indentidad incluido en ASP.NET Core. Como todo lo demas en el ecosistema de ASP.NET Core, es una serie de paquetes de NuGet que pueden ser instalados en cualquier proyecto (y están ya incluidos en la plantilla por defecto).

ASP.NET COre Indentity se encarga de guardar las cuentas de los usuarios asi como sifrar y guardar las claves de acceso asi como manejar los roles de los usuarios. Le provee apoyo a acceso mediante email y contraseña, autenticación de multi-factor, acceso con cuentas de redes sociales por proveedores como Google y Facebook asi como conección a otros servicios usando rpotocolos como OAuth 2.0 y OpenID Connect.

Las vistas de Register y Login que se incluyen con la plantilla de MVC + Individual Auth toman ventaja del ASP.NET COre Indentity y ya funcionan! Intenta registrar una cuenta y acceder usando la misma.

