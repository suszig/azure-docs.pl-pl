Użycie pliku parametrów można podać wartości parametrów podczas wdrażania, należy utworzyć plik JSON w formacie podobny do poniższego przykładu:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "webSiteName": {
            "value": "ExampleSite"
        },
        "webSiteHostingPlanName": {
            "value": "DefaultPlan"
        },
        "webSiteLocation": {
            "value": "West US"
        },
        "adminPassword": {
            "reference": {
               "keyVault": {
                  "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
               }, 
               "secretName": "sqlAdminPassword" 
            }   
        }
   }
}
```

Rozmiar pliku parametrów nie może być więcej niż 64 KB.

Jeśli trzeba podać poufne wartość dla parametru (na przykład hasło), należy dodać tę wartość do magazynu kluczy. Podczas wdrażania należy pobrać magazynu kluczy, jak pokazano w poprzednim przykładzie. Aby uzyskać więcej informacji, zobacz [przekazać wartości bezpieczne podczas wdrażania](../articles/azure-resource-manager/resource-manager-keyvault-parameter.md). 

