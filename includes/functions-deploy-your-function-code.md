## <a name="deploy-your-function-code"></a>Wdrażanie kodu funkcji  

Istnieje kilka sposobów tworzenia kodu funkcji w nowej aplikacji funkcji. Ten temat jest połączony z repozytorium przykładów w usłudze GitHub. Tak jak poprzednio, w poniższym kodzie w miejsce symbolu zastępczego `<app_name>` wstaw nazwę utworzonej aplikacji funkcji. 

```azurecli-interactive
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --branch master \
--repo-url https://github.com/Azure-Samples/functions-quickstart \
--manual-integration 
```
Po ustawieniu Źródło wdrożenia interfejsu wiersza polecenia Azure zawiera informacje podobne do poniższego przykładu (usunąć dla czytelności wartości null):

```json
{
  "branch": "master",
  "deploymentRollbackEnabled": false,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/...",
  "isManualIntegration": true,
  "isMercurial": false,
  "kind": null,
  "name": "quickstart",
  "repoUrl": "https://github.com/Azure-Samples/functions-quickstart",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.Web/sites/sourcecontrols"
}
```
