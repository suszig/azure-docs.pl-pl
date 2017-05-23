Użyj polecenia [az appservice web create](/cli/azure/appservice/web#create), aby utworzyć aplikację sieci Web. W następującym poleceniu wstaw unikatową nazwę aplikacji w miejsce symbolu zastępczego `<app_name>`. Aplikacja `<app_name>` jest używana w domyślnej witrynie DNS dla aplikacji internetowej. Jeśli nazwa `<app_name>` nie jest unikatowa, zostanie wyświetlony przyjazny komunikat o błędzie „Witryna internetowa o nazwie <app_name> już istnieje”.

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan quickStartPlan
```

Po utworzeniu aplikacji internetowej interfejs wiersza polecenia platformy Azure wyświetli informacje podobne do następujących:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "<app_name>.azurewebsites.net",
    "<app_name>.scm.azurewebsites.net"
  ],
  "gatewaySiteName": null,
  "hostNameSslStates": [
    {
      "hostType": "Standard",
      "name": "<app_name>.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "virtualIp": null
    }
    < JSON data removed for brevity. >
}
```

Przejdź do witryny (`http://<app_name>.azurewebsites.net`), aby zobaczyć nowo utworzoną aplikację internetową.

![app-service-web-service-created](../articles/app-service-web/media/app-service-web-get-started-nodejs-poc/app-service-web-service-created.png)


## <a name="configure-local-git-deployment"></a>Konfigurowanie lokalnego wdrożenia narzędzia Git

Usługa App Service obsługuje różne metody wdrażania zawartości w aplikacjach internetowych, np. protokół FTP, lokalne narzędzie Git oraz usługi GitHub, Visual Studio Team Services i Bitbucket. 

W ramach tego przewodnika Szybki start wdrożenie jest przeprowadzane przy użyciu lokalnego narzędzia Git. Oznacza to, że używa się polecenia Git do wypchnięcia z lokalnego repozytorium do repozytorium na platformie Azure. 

Za pomocą polecenia [az appservice web source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git) skonfiguruj dostęp lokalnego narzędzia Git do aplikacji internetowej.

```azurecli
az appservice web source-control config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Dane wyjściowe mają następujący format:

```
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

Zapisz wyświetlany identyfikator URI. Będzie on używany w następnym kroku. Zmienna `<username>` oznacza [użytkownika wdrożenia](#configure-a-deployment-user) utworzonego w poprzednim kroku.

## <a name="push-to-azure-from-git"></a>Wypychanie z narzędzia Git na platformę Azure

Dodaj zdalną platformę Azure do lokalnego repozytorium Git.

```bash
git remote add azure <URI from previous step>
```

Wypchnij na zdalną platformę Azure w celu wdrożenia aplikacji. Zostanie wyświetlony monit o podanie hasła utworzonego wcześniej podczas tworzenia użytkownika wdrożenia. Upewnij się, że zostało wprowadzone hasło utworzone podczas [konfiguracji użytkownika wdrożenia](#configure-a-deployment-user), a nie hasło, którego używasz do logowania się w witrynie Azure Portal.

```azurecli
git push azure master
```

Poprzednie polecenie wyświetli informacje podobne do następującego przykładu:

```bash
Counting objects: 13, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (11/11), done.
Writing objects: 100% (13/13), 2.07 KiB | 0 bytes/s, done.
Total 13 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'cc39b1e4cb'.
remote: Generating deployment script.
remote: Generating deployment script for Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

### <a name="browse-to-the-app"></a>Przechodzenie do aplikacji


Przejdź do wdrożonej aplikacji:

```
http://<app_name>.azurewebsites.net
```

