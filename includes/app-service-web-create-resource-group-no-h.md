W usłudze Cloud Shell utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create).

[!INCLUDE [resource group intro text](resource-group.md)]

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie *myResourceGroup* w lokalizacji *West Europe* (Europa Zachodnia).

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Zasadniczo grupy zasobów i zasoby są tworzone w pobliskim regionie. Aby wyświetlić wszystkie obsługiwane lokalizacje dla usługi Azure Web Apps, uruchom polecenie `az appservice list-locations`. 