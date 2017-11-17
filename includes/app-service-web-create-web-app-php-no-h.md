W powłoce chmury tworzenie aplikacji sieci web w `myAppServicePlan` planu usługi aplikacji z [tworzenie aplikacji sieci Web az](/cli/azure/webapp#create) polecenia. 

W poniższym przykładzie Zastąp `<app_name>` z globalnie unikatowej nazwy aplikacji (prawidłowe znaki to `a-z`, `0-9`, i `-`). Środowisko wykonawcze ma ustawioną wartość `PHP|7.0`. Aby wyświetlić wszystkie obsługiwanych środowisk uruchomieniowych, uruchom [az aplikacji sieci Web listy runtimes](/cli/azure/webapp#list-runtimes). 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "PHP|7.0" --deployment-local-git
```

Po utworzeniu aplikacji sieci web, Azure CLI przedstawia dane wyjściowe podobne do poniższego przykładu:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Zostanie utworzona pusta nowej aplikacji sieci web z włączonym wdrażaniem git.

> [!NOTE]
> Adres URL zdalnego Git jest wyświetlany w `deploymentLocalGitUrl` właściwości w formacie `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Zapisz ten adres URL, ponieważ będzie on potrzebny później.
>
