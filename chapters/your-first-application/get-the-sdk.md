## Obtener el SDK
Busca por "download .net core" y sigue las instrucciones que se encuentran en la página de bajada de Microsoft para tu plataforma. Luego que finalice la instalación del SDK, abre un Terminal (o PowerShell en Windows) y usa el commando `dotnet` (también llamadado **CLI** ) para verificar que todo esta funcionando:

```
dotnet --version

2.0.0
```

Puedes ver la información sobre tu plataforma con la opción `--info`:

```
dotnet --info

.NET Command Line Tools (2.0.0)

Product Information:
 Version:            2.0.0
 Commit SHA-1 hash:  cdcd1928c9

Runtime Environment:
 OS Name:     Mac OS X
 OS Version:  10.12

(more details...)
```

Si puedes ver los textos anteriores, estás listo para comenzar!