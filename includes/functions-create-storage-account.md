## <a name="create-an-azure-storage-account"></a>Tworzenie konta usługi Azure Storage

Funkcje używa ogólnego przeznaczenia konta w usłudze Azure Storage, aby zachować stan i inne informacje o funkcji. Utwórz konto magazynu ogólnego przeznaczenia w grupie zasobów został utworzony za pomocą [Tworzenie konta magazynu az](/cli/azure/storage/account#create) polecenia.

W poniższym poleceniu zastąp nazwę konta magazynu globalnie unikatowy, której występuje `<storage_name>` symbolu zastępczego. Nazwy kont usługi Magazyn muszą mieć długość od 3 do 24 znaków i mogą zawierać tylko cyfry i małe litery.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

Po utworzeniu konta magazynu w interfejsie wiersza polecenia platformy Azure zostanie wyświetlona informacja podobna do następującej:

```json
{
  "creationTime": "2017-04-15T17:14:39.320307+00:00",
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myresourcegroup/...",
  "kind": "Storage",
  "location": "westeurope",
  "name": "myfunctionappstorage",
  "primaryEndpoints": {
    "blob": "https://myfunctionappstorage.blob.core.windows.net/",
    "file": "https://myfunctionappstorage.file.core.windows.net/",
    "queue": "https://myfunctionappstorage.queue.core.windows.net/",
    "table": "https://myfunctionappstorage.table.core.windows.net/"
  },
     ....
    // Remaining output has been truncated for readability.
}
```