## <a name="prerequisites"></a>Wymagania wstępne

Jeśli jeszcze tego nie zrobiono, uzyskaj [bezpłatną wersję próbną subskrypcji Azure](https://azure.microsoft.com/pricing/free-trial/) i zainstaluj [interfejs wiersza polecenia Azure 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2).

## <a name="create-the-scale-set"></a>Tworzenie zestawu skalowania

Najpierw utwórz grupę zasobów, w której zostanie wdrożony zestaw skalowania:

```azurecli
az group create --location westus --name myResourceGroup
```

Teraz utwórz zestaw skalowania za pomocą polecenia `az vmss create`. Poniższy przykład obejmuje tworzenie zestawu skalowania systemu Linux o nazwie `myvmss` w grupie zasobów o nazwie `myrg`:

```azurecli
az vmss create --resource-group myResourceGroup --name myVmss \
    --image UbuntuLTS --admin-username azureuser \
    --authentication-type password --admin-password P4$$w0rd
```

Poniższy przykład tworzy zestaw skalowania systemu Windows z taką samą konfiguracją:

```azurecli
az vmss create --resource-group myResourceGroup --name myVmss \
    --image Win2016Datacenter --admin-username azureuser \
    --authentication-type password --admin-password P4$$w0rd
```

Jeśli chcesz wybrać inny obraz systemu operacyjnego, użyj polecenia `az vm image list` lub `az vm image list --all`, aby wyświetlić dostępne obrazy. Aby wyświetlić informacje o połączeniu dla maszyn wirtualnych w zestawie skalowania, użyj polecenia `az vmss list_instance_connection_info`:

```azurecli
az vmss list_instance_connection_info --resource-group myResourceGroup --name myVmss
```