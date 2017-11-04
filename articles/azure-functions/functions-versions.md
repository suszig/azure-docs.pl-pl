---
title: "Jak korzystać z wersji środowiska uruchomieniowego usługi Azure Functions | Dokumentacja firmy Microsoft"
description: "Funkcje obsługuje wiele wersji środowiska uruchomieniowego. Dowiedz się, jak określić wersję środowiska uruchomieniowego platformy Azure hostowanej funkcji aplikacji."
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2017
ms.author: glenga
ms.openlocfilehash: 26d4276a0a550d78a9c7657c464bd3320c956fb0
ms.sourcegitcommit: 54fd091c82a71fbc663b2220b27bc0b691a39b5b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/12/2017
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Jak korzystać z usługi Azure Functions wersji środowiska uruchomieniowego

Środowisko wykonawcze usługi Azure Functions implementuje niekorzystającą wykonywania kodu na platformie Azure. To środowisko uruchomieniowe można znaleźć w różnych środowiskach innych niż hostowana na platformie Azure. [Azure funkcje podstawowe narzędzia](functions-run-local.md) implementuje tego środowiska uruchomieniowego na komputerze deweloperskim. [Środowisko uruchomieniowe Functions Azure](functions-runtime-overview.md) pozwala używać funkcji w środowisku lokalnym. 

Funkcje obsługuje wiele wersji głównych środowiska uruchomieniowego. Aktualizacja wersji głównej można wprowadzać zmian, które psuły. W tym temacie opisano, jak można kierować przez aplikacje funkcja wersji określonego środowiska uruchomieniowego podczas hostowana na platformie Azure. 

Funkcje umożliwia docelowych określonych wersji głównej środowiska uruchomieniowego za pomocą `FUNCTIONS_EXTENSION_VERSION` ustawienia aplikacji w aplikacji funkcji. Dotyczy to zarówno publicznego i przejrzeć wersji. Funkcja aplikacji jest przechowywana na wersji głównych wykonawcze, dopóki nie wybierzesz jawnie można przenieść do nowej wersji. Użytkownik aplikacji funkcji zostało zaktualizowane do nowej wersji pomocniczych środowiska uruchomieniowego udostępniane. Wersja pomocnicza aktualizacje nie znajdą się istotne zmiany.  

Gdy publicznie dostępna jest nowa wersja główna, znajdują się możliwość Przenieś w górę do tej wersji podczas przeglądania aplikacji funkcji w portalu. Po przeniesieniu do nowej wersji, zawsze można użyć `FUNCTIONS_EXTENSION_VERSION` ustawienie aplikacji, aby powrócić do poprzedniej wersji środowiska uruchomieniowego.

Każda zmiana wersji środowiska uruchomieniowego powoduje, że funkcja aplikację, aby ponownie uruchomić. Informacje o wersji dla wszystkich wersji środowiska uruchomieniowego (główne i pomocnicze) są publikowane w [repozytorium GitHub](https://github.com/Azure/azure-webjobs-sdk-script/releases).   
## <a name="view-the-current-runtime-version"></a>Wyświetl bieżącą wersję środowiska uruchomieniowego

Poniższa procedura umożliwia wyświetlanie wersji wykonawczego specyficzne obecnie używana przez aplikację funkcji. 

1. W [portalu Azure](https://portal.azure.com), przejdź do aplikacji funkcji i w obszarze **skonfigurować funkcje**, wybierz **funkcji ustawienia aplikacji**. 

    ![Wybierz ustawienia aplikacji — funkcja](./media/functions-versions/add-update-app-setting.png)

2. W **funkcji ustawienia aplikacji** zlokalizuj **wersja środowiska uruchomieniowego**. Należy pamiętać, wersja określonego środowiska uruchomieniowego i żądanej wersji głównej. W poniższym przykładzie ustawiono wersji głównej `~1.0`.
 
   ![Wybierz ustawienia aplikacji — funkcja](./media/functions-versions/function-app-view-version.png)

## <a name="target-the-functions-version-20-runtime"></a>Celem funkcji środowiska wykonawczego w wersji 2.0

>[!IMPORTANT]   
> Środowisko uruchomieniowe Functions Azure 2.0 jest w wersji zapoznawczej i obecnie nie wszystkie funkcje usługi Azure Functions są obsługiwane. Aby uzyskać więcej informacji, zobacz [usługi Azure Functions runtime 2.0 znane problemy](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues)  

<!-- Add a table comparing the 1.x and 2.x runtime features-->

[!INCLUDE [functions-set-runtime-version](../../includes/functions-set-runtime-version.md)]

Funkcja aplikacji można przenieść do podglądu środowiska wykonawczego w wersji 2.0 w portalu Azure. W **funkcji ustawienia aplikacji** , wybierz pozycję **beta** w obszarze **wersja środowiska uruchomieniowego**.  

   ![Wybierz ustawienia aplikacji — funkcja](./media/functions-versions/function-app-view-version.png)

To ustawienie jest odpowiednikiem ustawienia `FUNCTIONS_EXTENSION_VERSION` ustawienie aplikacji, aby `beta`. Wybierz **~ 1** przycisk, aby powrócić do bieżącego publicznie obsługiwanych wersji głównej. Aby zaktualizować to ustawienie aplikacji umożliwia także wiersza polecenia platformy Azure. 

## <a name="target-a-specific-runtime-version-from-the-portal"></a>W docelowej wersji środowiska uruchomieniowego określonych w portalu

Kiedy należy określić wersję główną innej niż bieżąca głównych wersji lub wersji 2.0, należy ustawić `FUNCTIONS_EXTENSION_VERSION` ustawienia aplikacji.

1. W [portalu Azure](https://portal.azure.com), przejdź do aplikacji funkcji i w obszarze **skonfigurować funkcje**, wybierz **ustawienia aplikacji**.

    ![Wybierz ustawienia aplikacji — funkcja](./media/functions-versions/add-update-app-setting1a.png)

2. W **ustawienia aplikacji** karcie, Znajdź `FUNCTIONS_EXTENSION_VERSION` ustawienie i zmień wartość na prawidłową wersję główną środowiska uruchomieniowego 1.x lub `beta` w wersji 2.0. 

    ![Ustawienia aplikacji — funkcja](./media/functions-versions/add-update-app-setting2.png)

3. Kliknij przycisk **zapisać** można zapisać ustawienia aplikacji. 

## <a name="target-a-specific-version-using-azure-cli"></a>Docelowej określonej wersji przy użyciu wiersza polecenia platformy Azure

 Można również ustawić `FUNCTIONS_EXTENSION_VERSION` z wiersza polecenia platformy Azure. Przy użyciu wiersza polecenia platformy Azure, zaktualizuj ustawienia aplikacji w aplikacji funkcji z [az functionapp config appsettings zestaw](/cli/azure/functionapp/config/appsettings#set) polecenia.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```
W tym kodzie Zamień `<function_app>` z nazwą funkcji aplikacji. Zastąp również `<my_resource_group>` z nazwą grupy zasobów dla aplikacji funkcja. Zastąp `<version>` z nieprawidłową wersją główną 1.x środowiska uruchomieniowego lub `beta` w wersji 2.0. 

Możesz uruchomić to polecenie z [powłoki chmury Azure](../cloud-shell/overview.md) , wybierając **wypróbuj** w poprzednim przykładzie kodu. Można również użyć [lokalnie interfejsu wiersza polecenia Azure](/cli/azure/install-azure-cli) do wykonywania tego polecenia po wykonaniu [logowania az](/cli/azure#az_login) do logowania.
