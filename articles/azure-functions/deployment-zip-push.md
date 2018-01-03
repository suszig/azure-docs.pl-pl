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
ms.openlocfilehash: faddb73522200f60f18294dc43e8d235943f8bbb
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/20/2017
---
# <a name="zip-push-deployment-for-azure-functions"></a>ZIP wdrożenia wypychania dla usługi Azure Functions 
W tym artykule opisano sposób wdrażania funkcji plików projektu aplikacji Azure z pliku zip (skompresowane). Sposób na wdrożenie wypychania, zarówno przy użyciu wiersza polecenia platformy Azure i przy użyciu interfejsów API REST. 

Usługa Azure Functions oferuje pełną gamę ciągłego opcje wdrażania i integracji, które są dostarczane przez usługę Azure App Service. Aby uzyskać więcej informacji, zobacz [ciągłego wdrażania usługi Azure Functions](functions-continuous-deployment.md). 

Szybsze iteracji podczas tworzenia często jest łatwiejsze do wdrożenia funkcji pliki projektu aplikacji bezpośrednio z pliku zip skompresowane. To wdrożenie pliku zip korzysta z tej samej usługi Kudu tego uprawnienia ciągłej integracji na podstawie wdrożeń, w tym:

+ Usuwanie plików, które zostały utworzone podczas wcześniejszych wdrożeń.
+ Dostosowanie wdrożenia, łącznie z uruchamianiem skryptów wdrażania.
+ Dzienniki wdrożenia.
+ Synchronizowanie Funkcja Wyzwalacze w [planu zużycie](functions-scale.md) funkcji aplikacji.

Aby uzyskać więcej informacji, zobacz [odwołanie do wdrożenia wypychania .zip](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

## <a name="deployment-zip-file-requirements"></a>Wymagania dotyczące pliku zip wdrażania
Plik zip, służące do wdrażania wypychania musi zawierać wszystkie pliki projektu aplikacji funkcji, w tym kodu funkcji. 

>[!IMPORTANT]
> Użycie opcji wdrażania wypychania zip, pliki z istniejącego wdrożenia, które nie zostaną znalezione w pliku .zip są usuwane z funkcji aplikacji.  

### <a name="function-app-folder-structure"></a>Struktura folderów aplikacji — funkcja

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

### <a name="download-your-function-app-project"></a>Pobierz projekt aplikacji — funkcja

Gdy tworzysz na komputerze lokalnym, jest łatwo utworzyć plik .zip zawierający folder projektu aplikacji funkcji na komputerze deweloperskim. 

Jednak może utworzono funkcji za pomocą edytora w portalu Azure. Aby pobrać funkcja projektu aplikacji z portalu: 

1. Zaloguj się do [portalu Azure](https://portal.azure.com), a następnie przejdź do aplikacji funkcji.

2. Na **omówienie** wybierz opcję **pobierania zawartości aplikacji**. Wybierz swoje opcje pobierania, a następnie wybierz **Pobierz**.     

    ![Pobierz projekt aplikacji — funkcja](./media/deployment-zip-push/download-project.png)

Plik zip pobranego jest w poprawnym formacie należy opublikować ponownie aplikację funkcji przy użyciu wdrożenia wypychania zip.

Możesz również pobrać plik zip z repozytorium GitHub. Należy pamiętać, że podczas pobierania repozytorium GitHub jako plik .zip GitHub dodaje folder dodatkowy poziom dla gałęzi. Oznacza to, folder dodatkowego poziomu że nie można wdrożyć bezpośrednio jako plik zip pobrać z witryny GitHub. Jeśli używasz repozytorium GitHub do obsługi funkcji aplikacji, należy użyć [ciągłej integracji](functions-continuous-deployment.md) do wdrożenia aplikacji.  

## <a name="cli"></a>Wdrażanie przy użyciu wiersza polecenia platformy Azure

Można użyć wiersza polecenia platformy Azure, aby wyzwolić wdrożenie wypychania. Wypychania wdrażanie pliku zip do aplikacji funkcji przy użyciu [az functionapp wdrożenia źródła config-zip](/cli/azure/functionapp/deployment/source#az_functionapp_deployment_source_config_zip) polecenia. Aby użyć tego polecenia, należy użyć wiersza polecenia platformy Azure w wersji 2.0.21 lub nowszej. Aby wyświetlić jakiej używasz wersji interfejsu wiersza polecenia Azure, użyj `az --version` polecenia.

W poniższym poleceniu zastąp `<zip_file_path>` symbol zastępczy o ścieżkę do lokalizacji pliku zip. Ponadto Zastąp `<app_name>` o unikatowej nazwie funkcji aplikacji. 

```azurecli-interactive
az functionapp deployment source config-zip  -g myResourceGroup -n \
<app_name> --src <zip_file_path>
```
To polecenie wdraża pliki projektu z pliku zip pobranego do aplikacji funkcji na platformie Azure. Następnie uruchamia ponownie aplikację. Aby wyświetlić listę wdrożenia dla tej aplikacji funkcji, należy użyć interfejsów API REST.

Podczas korzystania z wiersza polecenia platformy Azure na komputerze lokalnym `<zip_file_path>` to ścieżka do pliku ZIP na tym komputerze. Można również uruchomić wiersza polecenia platformy Azure [powłoki chmury Azure](../cloud-shell/overview.md). Korzystając z powłoki w chmurze, możesz przekazać plik zip wdrożenia na koncie usługi pliki Azure skojarzoną z powłoki chmury. W takim przypadku `<zip_file_path>` jest lokalizacji magazynu, która używa konta powłoki chmury. Aby uzyskać więcej informacji, zobacz [utrwalić plików w powłoce chmury Azure](../cloud-shell/persisting-shell-storage.md).


[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]

[!INCLUDE [app-service-deploy-zip-push-custom](../../includes/app-service-deploy-zip-push-custom.md)]

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Ciągłe wdrażanie dla usługi Azure Functions](functions-continuous-deployment.md)

[.zip push deployment reference topic]: https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file
