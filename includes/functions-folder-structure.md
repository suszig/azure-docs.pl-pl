
Kod dla wszystkich funkcji w aplikacji określoną funkcję znajduje się w folderze głównym, który zawiera plik konfiguracji hosta i co najmniej jeden jego podfolderach. Każdy podfolder zawiera kod osobnych funkcji, jak w poniższym przykładzie:

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

Plik host.json zawiera niektóre konfiguracje specyficzne dla środowiska uruchomieniowego i znajduje się w folderze głównym aplikacji funkcji. Aby uzyskać informacje o ustawieniach, które są dostępne, zobacz [odwołania host.json](../articles/azure-functions/functions-host-json.md).

Każda funkcja ma folder, który zawiera jeden lub więcej plików kodu, konfiguracji function.json i innych zależności.

