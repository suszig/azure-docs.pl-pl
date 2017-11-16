---
title: Tworzenie pierwszej funkcji z poziomu interfejsu wiersza polecenia platformy Azure | Microsoft Docs
description: "Dowiedz się, jak utworzyć pierwszą funkcję platformy Azure do wykonywania bezserwerowego z poziomu interfejsu wiersza polecenia platformy Azure."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 11/08/2017
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: 4356d00b2694224f52a9359cd4a57d3a70a34d18
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="create-your-first-function-using-the-azure-cli"></a>Tworzenie pierwszej funkcji z poziomu interfejsu wiersza polecenia platformy Azure

W tym temacie szybkiego startu przedstawiono sposób użycia usługi Azure Functions można utworzyć swoją pierwszą funkcję. Użyj wiersza polecenia platformy Azure do tworzenia aplikacji funkcji, która jest [niekorzystającą](https://azure.microsoft.com/overview/serverless-computing/) infrastruktury, który jest hostem funkcji. Sam kod funkcji jest wdrażany z repozytorium przykładów GitHub.    

Poniższe kroki możesz wykonać przy użyciu komputera z systemem Mac, Windows lub Linux. 

## <a name="prerequisites"></a>Wymagania wstępne 

Przed uruchomieniem tego przykładu należy dysponować następującymi elementami:

+ Aktywne konto usługi [GitHub](https://github.com). 
+ Aktywna subskrypcja platformy Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli wybierzesz do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym temacie wymaga wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom `az --version` można znaleźć wersją. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 


[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app"></a>Tworzenie aplikacji funkcji

Do obsługi wykonywania funkcji potrzebna jest aplikacja funkcji. Aplikacja funkcji zapewnia środowisko do bezserwerowego wykonywania kodu funkcji. Umożliwia ona grupowanie funkcji w ramach jednostki logicznej, co ułatwia wdrażanie i udostępnianie zasobów oraz zarządzanie nimi. Utwórz aplikację funkcji przy użyciu polecenia [az functionapp create](/cli/azure/functionapp#create). 

W poniższym poleceniu Zastąp unikatowe funkcja Nazwa aplikacji, której występuje `<app_name>` symbolu zastępczego i konto magazynu nazwy `<storage_name>`. Nazwa `<app_name>` jest używana jako domyślna domena DNS aplikacji funkcji, więc nazwa ta musi być unikatowa wśród wszystkich aplikacji na platformie Azure. _Adres url źródła wdrożenia_ parametr to repozytorium przykładowej w usłudze GitHub, która zawiera funkcję "Hello World" HTTP wyzwolone.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--consumption-plan-location westeurope --deployment-source-url https://github.com/Azure-Samples/functions-quickstart
```
Ustawienie _zużycie plan lokalizacji_ parametru oznacza, że funkcja aplikacji znajduje się w plan hostingu zużycia. W tym planie zasoby są dodawane dynamicznie, co jest wymagane przez funkcji i płacisz tylko po uruchomieniu funkcji. Aby uzyskać więcej informacji, zobacz [Wybieranie odpowiedniego planu hostingu](functions-scale.md). 

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

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]