## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#az_group_create). Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure, takich jak aplikacje funkcji, bazy danych i konta magazynu, oraz zarządzania nimi.

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie `myResourceGroup`.  
Jeśli nie używasz powłoki chmury, zaloguj się przy pierwszym użyciu `az login`.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```
Zasadniczo grupy zasobów i zasoby są tworzone w pobliskim regionie. Aby wyświetlić wszystkie obsługiwane lokalizacje dla planów usługi App Service, uruchom [appservice az listy lokalizacje](/cli/azure/appservice#az_appservice_list_locations) polecenia.