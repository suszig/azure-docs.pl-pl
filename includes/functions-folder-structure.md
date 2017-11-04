
Kod dla wszystkich funkcji w danej funkcji aplikacji znajduje się w folderze głównym, który zawiera plik konfiguracji hosta i co najmniej jeden podfolderów, z których każdy zawiera kod osobnych funkcji, jak w poniższym przykładzie:

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

*Host.json* plik zawiera niektóre konfiguracji specyficznych dla środowiska uruchomieniowego i znajduje się w folderze głównym funkcji aplikacji. Aby uzyskać informacje na temat ustawień, które są dostępne, zobacz [odwołania host.json](../articles/azure-functions/functions-host-json.md).

Każda funkcja ma folder, który zawiera jeden lub więcej plików kodu, konfiguracji function.json i innych zależności.

