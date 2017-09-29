Za pomocą polecenia [az webapp create](/cli/azure/webapp#create) można utworzyć [aplikację internetową](../articles/app-service/app-service-web-overview.md) w planie usługi App Service `myAppServicePlan` w usłudze Cloud Shell. 

Aplikacja sieci Web udostępnia miejsce na hosting Twojego kodu i zawiera adres URL do wyświetlania wdrożonej aplikacji.

W poniższym poleceniu zastąp ciąg *\<nazwa_aplikacji>* unikatową nazwą (prawidłowe znaki to `a-z`, `0-9` i `-`). Jeśli nazwa `<app_name>` nie jest unikatowa, zostanie wyświetlony komunikat o błędzie „Witryna internetowa o nazwie <nazwa_aplikacji> już istnieje”. Domyślnym adresem URL aplikacji sieci Web jest `https://<app_name>.azurewebsites.net`. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan --deployment-local-git
```

Po utworzeniu aplikacji internetowej w interfejsie wiersza polecenia platformy Azure zostaną wyświetlone informacje podobne do następujących:

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

Przejdź do witryny, aby zobaczyć nowo utworzoną aplikację sieci Web.

```bash
http://<app_name>.azurewebsites.net
```
