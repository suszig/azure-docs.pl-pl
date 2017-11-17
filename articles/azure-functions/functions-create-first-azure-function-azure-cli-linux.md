---
title: Tworzenie pierwszej funkcji w systemie Linux z wiersza polecenia platformy Azure (wersja zapoznawcza) | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak utworzyć swoją pierwszą funkcję platformy Azure z domyślnego obrazu systemu Linux przy użyciu wiersza polecenia platformy Azure."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.date: 11/15/2017
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: d04e2000f2043e8bb11e15f6b9d7fd06ef5b9da3
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="create-your-first-function-running-on-linux-using-the-azure-cli-preview"></a>Tworzenie pierwszej funkcji uruchomiony w systemie Linux przy użyciu wiersza polecenia platformy Azure (wersja zapoznawcza)

Środowisko Azure Functions umożliwia hostowanie funkcji w systemie Linux w usłudze Azure App Service domyślnego kontenera. Ta funkcja jest obecnie w przeglądzie. Możesz również [Przynieś własne niestandardowe kontenera](functions-create-function-linux-custom-image.md). 

W tym temacie szybkiego startu przedstawiono sposób użycia usługi Azure Functions z wiersza polecenia platformy Azure do utworzenia pierwszej aplikacji funkcji w systemie Linux hostowanych na domyślny kontener usługi aplikacji. Funkcja kodu jest wdrażana na obraz z repozytorium GitHub próbki.    

Poniższe kroki są obsługiwane na komputerze Mac, systemu Windows lub Linux. 

## <a name="prerequisites"></a>Wymagania wstępne 

Aby ukończyć ten przewodnik Szybki Start, musisz spełnić następujące warunki:

+ Aktywna subskrypcja platformy Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli do zainstalowania i używania interfejsu wiersza polecenia lokalnie, w tym temacie, wymaga wiersza polecenia platformy Azure w wersji 2.0.21 lub nowszej. Uruchom `az --version` można znaleźć wersją. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Tworzenie planu usługi aplikacji systemu Linux

Linux obsługi funkcji jest obecnie obsługiwane tylko w planie usługi aplikacji. Zużycie plan hostingu nie jest jeszcze obsługiwany. Aby dowiedzieć się więcej o hostingu, zobacz [hosting usługi Azure Functions plany porównanie](functions-scale.md). 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-function-app-on-linux"></a>Tworzenie aplikacji funkcji w systemie Linux

Musi mieć aplikacji funkcji do hostowania wykonywanie funkcji w systemie Linux. Funkcja aplikacji udostępnia środowisko do wykonywania kodu funkcji. Umożliwia ona grupowanie funkcji w ramach jednostki logicznej, co ułatwia wdrażanie i udostępnianie zasobów oraz zarządzanie nimi. Tworzenia aplikacji funkcji za pomocą [az functionapp utworzyć](/cli/azure/functionapp#create) z planu usługi aplikacji systemu Linux. 

W poniższym poleceniu Zastąp unikatowe funkcja Nazwa aplikacji, której występuje `<app_name>` symbolu zastępczego i konto magazynu nazwy `<storage_name>`. Nazwa `<app_name>` jest używana jako domyślna domena DNS aplikacji funkcji, więc nazwa ta musi być unikatowa wśród wszystkich aplikacji na platformie Azure. _Adres url źródła wdrożenia_ parametr to repozytorium przykładowej w usłudze GitHub, która zawiera funkcję "Hello World" HTTP wyzwolone.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-source-url https://github.com/Azure-Samples/functions-quickstart-linux
```
Po nazwie funkcji aplikacji została utworzona i wdrożona, interfejsu wiersza polecenia Azure zawiera informacje podobne do poniższego przykładu:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
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

Ponieważ `myAppServicePlan` jest planu Linux obrazu wbudowanych docker służy do tworzenia kontenera, w którym działa funkcja aplikacji w systemie Linux. 

>[!NOTE]  
>Repozytorium przykładowej aktualnie obejmuje dwa pliki skryptów, [deploy.sh](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/deploy.sh) i [.deployment](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/.deployment). Plik .deployment Określa, że proces wdrażania umożliwia deploy.sh jako [niestandardowe wdrożenie skryptu](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script). W bieżącej wersji zapoznawczej skrypty są wymagane do wdrażania aplikacji funkcji w obrazie systemu Linux.  

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]
