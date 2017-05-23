---
title: Tworzenie pierwszej funkcji z poziomu interfejsu wiersza polecenia platformy Azure | Microsoft Docs
description: "Dowiedz się, jak utworzyć pierwszą funkcję platformy Azure do wykonywania bezserwerowego z poziomu interfejsu wiersza polecenia platformy Azure."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 05/02/2017
ms.topic: hero-article
ms.service: functions
ms.devlang: azure-cli
manager: erikre
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: ef9d65ae5ad0792230e1b8c0d7ed123c129f0f59
ms.contentlocale: pl-pl
ms.lasthandoff: 05/10/2017

---

# <a name="create-your-first-function-using-the-azure-cli"></a>Tworzenie pierwszej funkcji z poziomu interfejsu wiersza polecenia platformy Azure

W tym samouczku szybkiego startu omówiono tworzenie pierwszej funkcji przy użyciu usługi Azure Functions. Za pomocą interfejsu wiersza polecenia platformy Azure zostanie utworzona aplikacja funkcji, która jest bezserwerową infrastrukturą hostującą funkcję. Sam kod funkcji jest wdrażany z repozytorium przykładów GitHub.    

Poniższe kroki możesz wykonać przy użyciu komputera z systemem Mac, Windows lub Linux. Wykonanie wszystkich czynności opisanych w tym temacie powinno zająć jedynie około pięciu minut.

## <a name="prerequisites"></a>Wymagania wstępne 

Przed uruchomieniem tego przykładu należy dysponować następującymi elementami:

+ Aktywne konto usługi [GitHub](https://github.com). 
+ [Zainstalowany interfejs wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). 
+ Aktywna subskrypcja platformy Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do subskrypcji platformy Azure za pomocą polecenia [az login](/cli/azure/#login) i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie. 

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure, takich jak aplikacje funkcji, bazy danych i konta magazynu, oraz zarządzania nimi.

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie `myResourceGroup`:

```azurecli
az group create --name myResourceGroup --location westeurope
```
## <a name="create-an-azure-storage-account"></a>Tworzenie konta usługi Azure Storage

Usługa Functions przechowuje informacje o stanie i inne informacje dotyczące funkcji za pomocą konta usługi Azure Storage. Utwórz konto magazynu w utworzonej grupie zasobów przy użyciu polecenia [az storage account create](/cli/azure/storage/account#create).

W poniższym poleceniu w miejsce symbolu zastępczego `<storage_name>` wstaw swoją własną unikatową w skali globalnej nazwę konta magazynu. Nazwy kont usługi Magazyn muszą mieć długość od 3 do 24 znaków i mogą zawierać tylko cyfry i małe litery.

```azurecli
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

## <a name="create-a-function-app"></a>Tworzenie aplikacji funkcji

Do obsługi wykonywania funkcji potrzebna jest aplikacja funkcji. Aplikacja funkcji zapewnia środowisko do bezserwerowego wykonywania kodu funkcji. Umożliwia grupowanie funkcji jako jednostkę logiczną, co ułatwia wdrażanie i udostępnianie zasobów oraz zarządzanie nimi. Utwórz aplikację funkcji przy użyciu polecenia [az functionapp create](/cli/azure/functionapp#create). 

W poniższym poleceniu w miejsce symbolu zastępczego `<app_name>` wstaw swoją własną unikatową w skali globalnej nazwę aplikacji funkcji, a w miejsce symbolu zastępczego `<storage_name>` wstaw nazwę konta magazynu. Nazwa `<app_name>` jest używana jako domyślna domena DNS aplikacji funkcji, więc nazwa ta musi być unikatowa wśród wszystkich aplikacji na platformie Azure. 

```azurecli
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup --consumption-plan-location westeurope
```
Domyślnie aplikacja funkcji jest tworzona z planem hostingu Zużycie, co oznacza, że zasoby są dodawane dynamicznie zgodnie z wymaganiami funkcji, a opłaty są naliczane tylko wtedy, gdy funkcje są uruchomione. Aby uzyskać więcej informacji, zobacz [Wybieranie odpowiedniego planu hostingu](functions-scale.md). 

Po utworzeniu aplikacji funkcji interfejs wiersza polecenia platformy Azure wyświetli informacje podobne do następujących:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

Teraz, gdy masz już aplikację funkcji, możesz wdrożyć właściwy kod funkcji z repozytorium przykładów GitHub.

## <a name="deploy-your-function-code"></a>Wdrażanie kodu funkcji  

Istnieje kilka sposobów tworzenia kodu funkcji w nowej aplikacji funkcji. Ten temat jest połączony z repozytorium przykładów w usłudze GitHub. Tak jak poprzednio, w poniższym kodzie w miejsce symbolu zastępczego `<app_name>` wstaw nazwę utworzonej aplikacji funkcji. 

```azurecli
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --repo-url https://github.com/Azure-Samples/functions-quickstart --branch master --manual-integration
```
Po ustawieniu źródła wdrożenia w interfejsie wiersza polecenia platformy Azure zostanie wyświetlona informacja podobna do następującej (wartości null zostały usunięte, aby poprawić czytelność):

```json
{
  "branch": "master",
  "deploymentRollbackEnabled": false,
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myResourceGroup/...",
  "isManualIntegration": true,
  "isMercurial": false,
  "location": "West Europe",
  "name": "quickstart",
  "repoUrl": "https://github.com/Azure-Samples/functions-quickstart",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.Web/sites/sourcecontrols"
}
```

## <a name="test-the-function"></a>Testowanie funkcji

Przetestuj wdrożoną funkcję za pomocą programu cURL na komputerze Mac lub komputerze z systemem Linux bądź za pomocą powłoki Bash na komputerze z systemem Windows. Uruchom następujące polecenie programu cURL, wstawiając w miejsce symbolu zastępczego `<app_name>` nazwę aplikacji funkcji. Dołącz ciąg zapytania `&name=<yourname>` do adresu URL.

```bash
curl http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
```  

![Odpowiedź funkcji wyświetlona w przeglądarce.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-curl.png)  

Jeśli w wierszu polecenia nie jest dostępny program cURL, po prostu wprowadź ten sam adres URL w pasku adresu przeglądarki. Ponownie w miejsce symbolu zastępczego `<app_name>` wstaw nazwę aplikacji funkcji i dołącz ciąg zapytania `&name=<yourname>` do adresu URL, a następnie wykonaj żądanie. 

    http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
   
![Odpowiedź funkcji wyświetlona w przeglądarce.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-browser.png)  

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Inne przewodniki Szybki start w tej kolekcji bazują na tym przewodniku. Jeśli planujesz kontynuować pracę z kolejnymi przewodnikami Szybki start lub samouczkami, nie usuwaj zasobów utworzonych w tym przewodniku Szybki start. Jeśli nie planujesz kontynuować pracy, użyj poniższego polecenia, aby usunąć wszystkie zasoby utworzone w ramach tego przewodnika Szybki start:

```azurecli
az group delete --name myResourceGroup
```
Gdy pojawi się monit, wpisz `y`.

## <a name="next-steps"></a>Następne kroki

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

