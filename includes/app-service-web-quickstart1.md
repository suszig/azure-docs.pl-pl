## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

 [Grupa zasobów platformy Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) to logiczny kontener, w którym są zarządzane takie zasoby, jak aplikacje sieci Web i bazy danych. W grupie zasobów możesz wdrażać, aktualizować i usuwać zasoby.

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create).

```azurecli
az group create --name myResourceGroup --location westeurope
```

Aby wyświetlić dostępne lokalizacje, użyj polecenia `az appservice list-locations`. Zasadniczo tworzysz zasoby w pobliskim regionie.

## <a name="create-an-azure-app-service-plan"></a>Tworzenie planu usługi Azure App Service

Utwórz „BEZPŁATNY” [plan usługi App Service](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) za pomocą polecenia [az appservice plan create](/cli/azure/appservice/plan#create).

[!INCLUDE [app-service-plan](app-service-plan.md)]

Następujące polecenie tworzy plan usługi App Service o nazwie `quickStartPlan` przy użyciu warstwy cenowej **Bezpłatna**.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku FREE
```

Po utworzeniu planu usługi App Service interfejs wiersza polecenia platformy Azure wyświetli informacje podobne do następujących:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "quickStartPlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
  "kind": "app",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "quickStartPlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```

## <a name="create-a-web-app"></a>Tworzenie aplikacji sieci Web