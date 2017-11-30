W usłudze Cloud Shell utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#az_group_create).

[!INCLUDE [resource group intro text](resource-group.md)]

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *West Europe* (Europa Zachodnia). Aby wyświetlić wszystkie obsługiwane lokalizacje dla aplikacji usługi, uruchom `az appservice list-locations` polecenia.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Zasadniczo grupy zasobów i zasoby są tworzone w pobliskim regionie. 
