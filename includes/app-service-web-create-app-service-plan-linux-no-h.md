W powłoce chmury, Utwórz plan usługi aplikacji w grupie zasobów z [Tworzenie planu usług aplikacji az](/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create) polecenia.

<!-- [!INCLUDE [app-service-plan](app-service-plan-linux.md)] -->

Poniższy przykład tworzy plan usługi aplikacji o nazwie `myAppServicePlan` w **standardowe** warstwy cenowej (`--sku S1`) i w kontenerze systemu Linux (`--is-linux`).

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

Po utworzeniu planu usługi App Service interfejs wiersza polecenia platformy Azure wyświetli informacje podobne do następujących:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "linux",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```
