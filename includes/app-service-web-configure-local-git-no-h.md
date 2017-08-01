Skonfiguruj lokalne wdrożenie Git do aplikacji internetowej za pomocą polecenia [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#config-local-git).

Usługa App Service obsługuje różne metody wdrażania zawartości w aplikacjach internetowych, np. protokół FTP, lokalne narzędzie Git oraz usługi GitHub, Visual Studio Team Services i Bitbucket. W ramach tego przewodnika Szybki start wdrożenie jest przeprowadzane przy użyciu lokalnego narzędzia Git. Oznacza to, że używa się polecenia Git do wypchnięcia z lokalnego repozytorium do repozytorium na platformie Azure. 

W poniższym poleceniu zastąp ciąg *\<nazwa_aplikacji>* nazwą swojej aplikacji internetowej.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Dane wyjściowe mają następujący format:

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

Zmienna `<username>` oznacza [użytkownika wdrożenia](#configure-a-deployment-user) utworzonego w poprzednim kroku.

Skopiuj pokazany identyfikator URI; zostanie on użyty w następnym kroku.
