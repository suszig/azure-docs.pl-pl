---
title: "ZIP wdrożenia wypychania dla usługi Azure Functions | Dokumentacja firmy Microsoft"
description: "Publikowanie funkcji platformy Azure przy użyciu urządzenia wdrożenia pliku zip Kudu wdrożenia usługi."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/06/2017
ms.author: glenga
ms.openlocfilehash: 5c8c608126f20aa5f5dc52bb8e24cfec14fd00f5
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="zip-push-deployment-for-azure-functions"></a>ZIP wdrożenia wypychania dla usługi Azure Functions 
W tym temacie opisano sposób wdrażania funkcji plików projektu aplikacji Azure z pliku zip (skompresowane). Sposób na wdrożenie wypychania, używając wiersza polecenia platformy Azure i interfejsów API REST. Usługa Azure Functions oferuje pełną gamę ciągłego wdrażania i opcje integracji dostarczone przez usługę Azure App Service, aby uzyskać więcej informacji, zobacz [ciągłego wdrażania usługi Azure Functions](functions-continuous-deployment.md). Jednak dla szybsze iteracji podczas tworzenia, jest często łatwiejsze do wdrożenia funkcji pliki projektu aplikacji bezpośrednio z pliku zip skompresowane.

To wdrożenie pliku zip używa tej samej usługi Kudu tego uprawnienia ciągłej integracji wdrożenia oparte na, która obejmuje następujące funkcje:

+ Usuwanie plików pozostałość z poprzedniego wdrożenia.
+ Dostosowanie wdrożenia, łącznie z uruchamianiem skryptów wdrażania.
+ Dzienniki wdrożenia.
+ Synchronizowanie Funkcja Wyzwalacze w [planu zużycie](functions-scale.md) funkcji aplikacji.

Aby uzyskać więcej informacji, zobacz [temat referencyjny wdrożenia wypychania .zip]. 

## <a name="requirements-of-the-deployment-zip-file"></a>Wymagania dotyczące pliku .zip wdrożenia 
Plik zip, używane w przypadku wdrażania wypychania musi zawiera wszystkie pliki projektu aplikacji funkcji, w tym kodu funkcji. 

>[!IMPORTANT]
> Użycie opcji wdrażania wypychania zip, wszystkie pliki z istniejącego wdrożenia nie można odnaleźć w pliku .zip są usuwane z funkcji aplikacji.  

### <a name="function-app-folder-structure"></a>Struktura folderów aplikacji — funkcja

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

### <a name="downloading-your-function-app-project"></a>Pobieranie projektu aplikacji — funkcja

Podczas wykonywania rozwoju lokalnego jest łatwo utworzyć plik .zip zawierający folder projektu aplikacji funkcji na komputerze deweloperskim. Jednak może również utworzono funkcji za pomocą edytora w portalu. Aby pobrać funkcja projektu aplikacji z portalu: 

1. Zaloguj się do [portalu Azure](https://portal.azure.com) i przejdź do aplikacji funkcji.

2. W **omówienie** wybierz opcję **pobierania zawartości aplikacji**, a następnie wybierz opcje pobierania i kliknij przycisk **Pobierz**.     

    ![Pobierz projekt aplikacji — funkcja](./media/deployment-zip-push/download-project.png)

Plik zip pobranego jest w poprawnym formacie należy opublikować ponownie do aplikacji funkcji przy użyciu wdrożenia wypychania zip.

GitHub umożliwia również pobrać plik zip z repozytorium. Należy pamiętać, że podczas pobierania repozytorium GitHub jako plik .zip GitHub dodaje dodatkowy folder poziomu dla gałęzi, co oznacza, że nie można wdrożyć pliku zip pobrane z usługi GitHub, ponieważ jest. Jeśli korzystasz z repozytorium GitHub do obsługi funkcji aplikacji, należy użyć [ciągłej integracji](functions-continuous-deployment.md) do wdrożenia aplikacji.  

## <a name="cli"></a>Wdrażanie przy użyciu wiersza polecenia platformy Azure

Można użyć wiersza polecenia platformy Azure, aby wyzwolić wdrożenie wypychania. Wypychania wdrażanie pliku zip do aplikacji funkcji przy użyciu [az functionapp wdrożenia źródła config-zip](/cli/azure/functionapp/deployment/source#az_functionapp_deployment_source_config_zip) polecenia. Należy użyć wiersza polecenia platformy Azure w wersji 2.0.21 lub nowszej. Użyj `az --version` polecenie, aby wyświetlić jakiej wersji korzystasz.

W poniższym poleceniu zastąp `<zip_file_path>` symbol zastępczy o ścieżkę do lokalizacji pliku zip. Ponadto Zastąp `<app_name>` o unikatowej nazwie funkcji aplikacji. 

```azurecli-interactive
az functionapp deployment source config-zip  -g myResourceGroup -n \
<app_name> --src <zip_file_path>
```
To polecenie wdraża pliki projektu z pliku zip pobranego do aplikacji funkcji na platformie Azure i ponownego uruchomienia aplikacji. Aby wyświetlić listę wdrożenia dla tej aplikacji funkcji, należy użyć interfejsów API REST.

Jeśli używasz interfejsu wiersza polecenia Azure na komputerze lokalnym `<zip_file_path>` to ścieżka do pliku ZIP na tym komputerze. Można również uruchomić wiersza polecenia platformy Azure [powłoki chmury Azure](../cloud-shell/overview.md). Za pomocą powłoki w chmurze, należy najpierw przekazać pliku zip wdrożenia, do konta Azure plików skojarzonych z powłoki chmury. W takim przypadku `<zip_file_path>` jest lokalizacja magazynu używany przez Twoje konto powłoki chmury. Aby uzyskać więcej informacji, zobacz [utrwalić plików w powłoce chmury Azure](../cloud-shell/persisting-shell-storage.md).


[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]

[!INCLUDE [app-service-deploy-zip-push-custom](../../includes/app-service-deploy-zip-push-custom.md)]

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Ciągłe wdrażanie dla usługi Azure Functions](functions-continuous-deployment.md)

[temat referencyjny wdrożenia wypychania .zip]: https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file
