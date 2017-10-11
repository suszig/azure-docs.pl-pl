---
title: "Link szablonów dla wdrożenia usługi Azure | Dokumentacja firmy Microsoft"
description: "Informacje dotyczące używania szablonów połączonych w szablonie usługi Azure Resource Manager tworzenie rozwiązań moduły szablonu. Pokazuje, jak można przekazać wartości parametrów, określ plik parametrów i dynamicznie utworzone adresy URL."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 27d8c4b2-1e24-45fe-88fd-8cf98a6bb2d2
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: tomfitz
ms.openlocfilehash: 8b58a83ffd473500dd3f76c09e251f9208527d4f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="using-linked-templates-when-deploying-azure-resources"></a>Korzystanie z szablonów połączonych w przypadku wdrażania zasobów platformy Azure
Z jednego szablonu usługi Azure Resource Manager, można połączyć z innego szablonu, który umożliwia dekompozycji wdrożenia do zestawu z celem, szablony specyficzne dla celu. Tak jak w przypadku decomposing aplikację na kilku kod klasy, dekompozycji zapewnia korzyści w zakresie testowania, ponownemu i czytelność.  

Można przekazać do połączonego szablonu parametry z głównym szablonu, a tych parametrów można bezpośrednio mapowania parametrów i zmiennych w szablonie wywoływania. Połączone szablonu można również przekazać do zmiennej dane wyjściowe do szablonu źródła włączenie wymiany danych dwukierunkowej między szablonami.

## <a name="linking-to-a-template"></a>Łączenie z szablonu
Możesz utworzyć łącza między dwa szablony, dodając zasobu wdrożenia w szablonie głównym wskazujące połączonego szablonu. Możesz ustawić **templateLink** właściwości do identyfikatora URI połączonego szablonu. Można podać wartości parametrów szablonu połączone, bezpośrednio w szablonie lub w pliku parametrów. W poniższym przykładzie użyto **parametry** właściwości w celu określenia wartości parametru bezpośrednio.

```json
"resources": [ 
  { 
      "apiVersion": "2017-05-10", 
      "name": "linkedTemplate", 
      "type": "Microsoft.Resources/deployments", 
      "properties": { 
        "mode": "incremental", 
        "templateLink": {
          "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion": "1.0.0.0"
        }, 
        "parameters": { 
          "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
        } 
      } 
  } 
] 
```

Podobnie jak inne typy zasobów można ustawić zależności między połączonego szablonu i innych zasobów. W związku z tym inne zasoby potrzebują wartość wyjściowa szablonu połączone, można upewnij się, że połączonego szablonu jest wdrożyć przed ich. Lub, gdy szablon połączonego opiera się na inne zasoby, można upewnij się, że inne zasoby są wdrażane przed połączonego szablonu. Można pobrać wartości z połączonych szablonu przy użyciu następującej składni:

```json
"[reference('linkedTemplate').outputs.exampleProperty.value]"
```

Usługa Resource Manager musi mieć możliwość dostępu do połączonego szablonu. Nie można określić plik lokalny lub plik, który jest dostępny tylko w sieci lokalnej połączonego szablonu. Możesz udostępniać wartość identyfikatora URI, która zawiera jedną **http** lub **https**. Jedną z opcji jest umieszczenie szablonu połączonego na koncie magazynu i użyj identyfikatora URI dla tego elementu, tak jak pokazano w poniższym przykładzie:

```json
"templateLink": {
    "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
    "contentVersion": "1.0.0.0",
}
```

Mimo że połączonego szablonu musi być dostępny zewnętrznie, nie trzeba być ogólnie dostępne publicznie. Możesz dodać do szablonu na konto magazynu prywatnego, który jest dostępny tylko dla właściciela konta magazynu. Następnie można utworzyć token sygnatury dostępu Współdzielonego dostępu współdzielonego, aby umożliwić dostęp podczas wdrażania. Identyfikator URI dla połączonych szablonu należy dodać tokenu sygnatury dostępu Współdzielonego. Aby uzyskać instrukcje na temat ustawiania szablonu na koncie magazynu i generowania tokenu sygnatury dostępu Współdzielonego, zobacz [wdrażanie zasobów przy użyciu szablonów usługi Resource Manager i programu Azure PowerShell](resource-group-template-deploy.md) lub [wdrożenie zasobów z szablonami usługi Resource Manager i interfejsu wiersza polecenia Azure](resource-group-template-deploy-cli.md). 

W poniższym przykładzie przedstawiono szablonu nadrzędnego, który stanowi łącze do innego szablonu. Połączony jest uzyskiwany z tokenu sygnatury dostępu Współdzielonego, który jest przekazywana jako parametr.

```json
"parameters": {
    "sasToken": { "type": "securestring" }
},
"resources": [
    {
        "apiVersion": "2017-05-10",
        "name": "linkedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
          "mode": "incremental",
          "templateLink": {
            "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
            "contentVersion": "1.0.0.0"
          }
        }
    }
],
```

Nawet jeśli token jest przekazywany jako bezpieczny ciąg, identyfikator URI szablonu połączone, wraz z tokenem sygnatury dostępu Współdzielonego są rejestrowane w operacji wdrażania. W celu ograniczenia narażenia, ustawienia okresu ważności tokenu.

Menedżer zasobów obsługuje każdego połączonego szablonu jako osobne wdrożenia. W historii wdrożenia dla grupy zasobów zobacz temat oddzielnych wdrożeń nadrzędny i zagnieżdżone szablony.

![historia wdrażania](./media/resource-group-linked-templates/linked-deployment-history.png)

## <a name="linking-to-a-parameter-file"></a>Łączenie z pliku parametrów
W następnym przykładzie użyto **parametersLink** właściwości, aby utworzyć link do pliku parametrów.

```json
"resources": [ 
  { 
     "apiVersion": "2017-05-10", 
     "name": "linkedTemplate", 
     "type": "Microsoft.Resources/deployments", 
     "properties": { 
       "mode": "incremental", 
       "templateLink": {
          "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
          "contentVersion":"1.0.0.0"
       }, 
       "parametersLink": { 
          "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
          "contentVersion":"1.0.0.0"
       } 
     } 
  } 
] 
```

Wartość identyfikatora URI dla pliku połączonego parametru nie może być lokalny plik i musi zawierać albo **http** lub **https**. Można też maksymalnie dostęp za pośrednictwem tokenu sygnatury dostępu Współdzielonego pliku parametrów.

## <a name="using-variables-to-link-templates"></a>Użycie zmiennych połączenia szablonów
W poprzednich przykładach pokazano zakodowanych wartości adresu URL dla łączy szablonu. Takie podejście może działać w przypadku prostego szablonu, ale nie działa w przypadku pracy z dużym zestawem moduły szablonów. Zamiast tego można utworzyć zmienną statyczną, przechowującym bazowy adres URL dla szablonu głównego, a następnie dynamicznie utworzyć adresów URL dla szablonów połączonych z tym podstawowego adresu URL. Zaletą tej metody jest można łatwo przenosić lub rozwidlania szablonu, ponieważ musisz zmienić zmienna statyczna w szablonie głównym. Główny szablon przekazuje prawidłowe identyfikatory URI w szablonie rozłożone.

Poniższy przykład przedstawia sposób Użyj podstawowego adresu URL, aby utworzyć dwa adresy URL dla szablonów połączonych (**sharedTemplateUrl** i **vmTemplate**). 

```json
"variables": {
    "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
    "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
    "vmTemplateUrl": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]"
}
```

Można również użyć [deployment()](resource-group-template-functions-deployment.md#deployment) uzyskać podstawowy adres URL dla bieżącego szablonu i używać, aby uzyskać adres URL dla innych szablonów w tej samej lokalizacji. Ta metoda jest przydatna, jeśli zmieni się lokalizację szablonu (być może z powodu versioning) lub aby uniknąć twardego kodowania adresów URL w pliku szablonu. 

```json
"variables": {
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
}
```

## <a name="complete-example"></a>Pełny przykład
Następujące szablony przykład Pokaż uproszczony rozmieszczenie szablonów połączonych aby zilustrować niektóre pojęcia w tym artykule. Przyjęto założenie, że szablony zostały dodane do tego samego kontenera na koncie magazynu o dostępie wyłączone. Szablon połączonego przekazuje wartość z powrotem na główny szablonu w **generuje** sekcji.

**Parent.json** plik zawiera:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerSasToken": { "type": "string" }
  },
  "resources": [
    {
      "apiVersion": "2017-05-10",
      "name": "linkedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
          "contentVersion": "1.0.0.0"
        }
      }
    }
  ],
  "outputs": {
    "result": {
      "type": "string",
      "value": "[reference('linkedTemplate').outputs.result.value]"
    }
  }
}
```

**Helloworld.json** plik zawiera:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [],
  "outputs": {
    "result": {
        "value": "Hello World",
        "type" : "string"
    }
  }
}
```

W programie PowerShell możesz uzyskać token dla kontenera i wdrażać szablonów:

```powershell
Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
$token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
$url = (Get-AzureStorageBlob -Container templates -Blob parent.json).ICloudBlob.uri.AbsoluteUri
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ($url + $token) -containerSasToken $token
```

W programie Azure CLI 2.0 uzyskać token dla kontenera i wdrażanie szablonów z następującym kodem:

```azurecli
expiretime=$(date -u -d '30 minutes' +%Y-%m-%dT%H:%MZ)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name storagecontosotemplates \
    --query connectionString)
token=$(az storage container generate-sas \
    --name templates \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name parent.json \
    --output tsv \
    --connection-string $connection)
parameter='{"containerSasToken":{"value":"?'$token'"}}'
az group deployment create --resource-group ExampleGroup --template-uri $url?$token --parameters $parameter
```

## <a name="next-steps"></a>Następne kroki
* Aby dowiedzieć się więcej na temat definiowania kolejność wdrażania zasobów, zobacz [Definiowanie zależności w szablonach usługi Azure Resource Manager](resource-group-define-dependencies.md)
* Aby dowiedzieć się, jak zdefiniować jeden zasób, ale utworzenia wielu wystąpień, zobacz [utworzyć wiele wystąpień zasobów usługi Azure Resource Manager](resource-group-create-multiple.md)

