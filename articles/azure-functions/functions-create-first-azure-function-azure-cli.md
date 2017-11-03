---
title: Tworzenie pierwszej funkcji z poziomu interfejsu wiersza polecenia platformy Azure | Microsoft Docs
description: "Dowiedz się, jak utworzyć pierwszą funkcję platformy Azure do wykonywania bezserwerowego z poziomu interfejsu wiersza polecenia platformy Azure."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 08/22/2017
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: ab35963dc9d10134799270e6ab3e6593be0e601a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-function-using-the-azure-cli"></a>Tworzenie pierwszej funkcji z poziomu interfejsu wiersza polecenia platformy Azure

W tym samouczku szybkiego startu omówiono tworzenie pierwszej funkcji przy użyciu usługi Azure Functions. Za pomocą interfejsu wiersza polecenia platformy Azure zostanie utworzona aplikacja funkcji, która jest bezserwerową infrastrukturą hostującą funkcję. Sam kod funkcji jest wdrażany z repozytorium przykładów GitHub.    

Poniższe kroki możesz wykonać przy użyciu komputera z systemem Mac, Windows lub Linux. 

## <a name="prerequisites"></a>Wymagania wstępne 

Przed uruchomieniem tego przykładu należy dysponować następującymi elementami:

+ Aktywne konto usługi [GitHub](https://github.com). 
+ Aktywna subskrypcja platformy Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym temacie wymaga wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom `az --version` można znaleźć wersją. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#create). Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure, takich jak aplikacje funkcji, bazy danych i konta magazynu, oraz zarządzania nimi.

Poniższy przykład obejmuje tworzenie grupy zasobów o nazwie `myResourceGroup`.  
Jeśli nie używasz powłoki chmury, zaloguj się przy pierwszym użyciu `az login`.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```


## <a name="create-an-azure-storage-account"></a>Tworzenie konta usługi Azure Storage

Usługa Functions przechowuje informacje o stanie i inne informacje dotyczące funkcji za pomocą konta usługi Azure Storage. Utwórz konto magazynu w utworzonej grupie zasobów przy użyciu polecenia [az storage account create](/cli/azure/storage/account#create).

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

## <a name="create-a-function-app"></a>Tworzenie aplikacji funkcji

Do obsługi wykonywania funkcji potrzebna jest aplikacja funkcji. Aplikacja funkcji zapewnia środowisko do bezserwerowego wykonywania kodu funkcji. Umożliwia ona grupowanie funkcji w ramach jednostki logicznej, co ułatwia wdrażanie i udostępnianie zasobów oraz zarządzanie nimi. Utwórz aplikację funkcji przy użyciu polecenia [az functionapp create](/cli/azure/functionapp#create). 

W poniższym poleceniu Zastąp unikatowe funkcja Nazwa aplikacji, której występuje `<app_name>` symbolu zastępczego i konto magazynu nazwy `<storage_name>`. Nazwa `<app_name>` jest używana jako domyślna domena DNS aplikacji funkcji, więc nazwa ta musi być unikatowa wśród wszystkich aplikacji na platformie Azure. 

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--consumption-plan-location westeurope
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

Jeśli w wierszu polecenia nie jest dostępny program cURL, wprowadź ten sam adres URL w pasku adresu przeglądarki. W miejsce symbolu zastępczego `<app_name>` wstaw nazwę aplikacji funkcji i dołącz ciąg zapytania `&name=<yourname>` do adresu URL, a następnie wykonaj żądanie. 

    http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
   
![Odpowiedź funkcji wyświetlona w przeglądarce.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-browser.png)  

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Inne przewodniki Szybki start w tej kolekcji bazują na tym przewodniku. Jeśli planujesz kontynuować pracę z kolejnymi przewodnikami Szybki start lub samouczkami, nie usuwaj zasobów utworzonych w tym przewodniku Szybki start. Jeśli nie planujesz kontynuować pracy, użyj poniższego polecenia, aby usunąć wszystkie zasoby utworzone w ramach tego przewodnika Szybki start:

```azurecli-interactive
az group delete --name myResourceGroup
```
Gdy pojawi się monit, wpisz `y`.

## <a name="next-steps"></a>Następne kroki

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]
