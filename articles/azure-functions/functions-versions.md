---
title: "Jak korzystać z usługi Azure Functions wersji środowiska uruchomieniowego"
description: "Środowisko Azure Functions obsługuje wiele wersji środowiska uruchomieniowego. Dowiedz się, jak określić wersję środowiska uruchomieniowego aplikacji funkcji obsługiwane na platformie Azure."
services: functions
documentationcenter: 
author: ggailey777
manager: cfowler
editor: 
ms.service: functions
ms.workload: na
ms.devlang: na
ms.topic: article
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: 3f816f661767d2e372b02b207d6fa7efd494e6ec
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/29/2018
---
# <a name="how-to-target-azure-functions-runtime-versions"></a>Jak korzystać z usługi Azure Functions wersji środowiska uruchomieniowego

Aplikacja funkcji jest uruchamiane w określonej wersji środowiska uruchomieniowego usługi Azure Functions. Istnieją dwa główne wersje: 1.x i 2.x. W tym artykule wyjaśniono, jak wybrać Wybierz wersję główną do użycia oraz sposób konfigurowania aplikacji funkcji na platformie Azure do uruchomienia w wersji. Aby uzyskać informacje o sposobie konfigurowania lokalne Środowisko deweloperskie dla określonej wersji, zobacz [kodu oraz testów usługi Azure Functions lokalnie](functions-run-local.md).

## <a name="differences-between-runtime-1x-and-2x"></a>Różnice między środowiska uruchomieniowego 1.x i 2.x

> [!IMPORTANT] 
> Środowisko uruchomieniowe 1.x jest tylko wersja zatwierdzone do użycia w środowisku produkcyjnym.

| Środowisko uruchomieniowe | Stan |
|---------|---------|
|1.x|Ogólnie dostępna (GA)|
|2.x|Wersja zapoznawcza|

W poniższych sekcjach opisano różnice w językach, powiązania oraz Obsługa programowania i platform.

### <a name="languages"></a>Języki

W poniższej tabeli przedstawiono, które języki programowania są obsługiwane w każdej wersji środowiska wykonawczego.

[!INCLUDE [functions-supported-languages](../../includes/functions-supported-languages.md)]

Aby uzyskać więcej informacji, zobacz [obsługiwanych języków](supported-languages.md).

### <a name="bindings"></a>Powiązania 

Środowisko uruchomieniowe 2.x umożliwia tworzenie niestandardowych [powiązanie rozszerzenia](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Binding-Extensions-Overview). Wbudowane powiązania, które używają tego modelu rozszerzalności są dostępne tylko w 2.x; wśród pierwszy z nich jest [powiązania Microsoft Graph](functions-bindings-microsoft-graph.md).

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Aby uzyskać więcej informacji dotyczących powiązań pomocy technicznej i inne przerwy funkcjonalności w 2.x, zobacz [Runtime 2.0 znane problemy](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues).

### <a name="cross-platform-development"></a>Wiele platform

Środowisko uruchomieniowe 1.x obsługuje funkcja programowanie tylko w portalu lub w systemie Windows; z 2.x mogą tworzyć i uruchamiać funkcje platformy Azure w systemie Linux lub macOS.

## <a name="automatic-and-manual-version-updates"></a>Aktualizacje automatycznej i ręcznej wersji

Funkcje umożliwia docelowych przy użyciu określonej wersji środowiska uruchomieniowego `FUNCTIONS_EXTENSION_VERSION` ustawienia aplikacji w aplikacji funkcji. Funkcja aplikacji jest przechowywana na określonej wersji głównej, dopóki nie wybierzesz jawnie można przenieść do nowej wersji.

Jeśli określisz tylko wersję główną ("~ 1" dla 1.x) lub "beta" dla 2.x aplikacji funkcji jest automatycznie aktualizowane do nowej wersji pomocniczych środowiska uruchomieniowego udostępniane. Nowe wersje pomocnicze nie znajdą się istotne zmiany. Jeśli określono wersję pomocniczą (na przykład "1.0.11360"), dopóki jawnie zmienić aplikacji funkcji jest przechowywana w tej wersji. 

Gdy publicznie dostępna jest nowa wersja, monitu w aplikacji portal daje możliwość Przenieś w górę do tej wersji. Po przeniesieniu do nowej wersji, zawsze można użyć `FUNCTIONS_EXTENSION_VERSION` ustawienie aplikacji, aby powrócić do poprzedniej wersji.

Zmiana wersji środowiska uruchomieniowego powoduje, że ponowne uruchomienie aplikacji funkcji.

Wartości można ustawić w `FUNCTIONS_EXTENSION_VERSION` aplikacji ustawienie, aby włączyć Aktualizacje automatyczne są obecnie "~ 1" dla środowiska uruchomieniowego 1.x i wersji "beta" dla 2.x.

## <a name="view-the-current-runtime-version"></a>Wyświetl bieżącą wersję środowiska uruchomieniowego

Poniższa procedura umożliwia wyświetlanie wersji środowiska uruchomieniowego aktualnie używany przez aplikację funkcji. 

1. W [portalu Azure](https://portal.azure.com), przejdź do aplikacji funkcji i w obszarze **skonfigurować funkcje**, wybierz **funkcji ustawienia aplikacji**. 

    ![Wybierz ustawienia aplikacji — funkcja](./media/functions-versions/add-update-app-setting.png)

2. W **funkcji ustawienia aplikacji** zlokalizuj **wersja środowiska uruchomieniowego**. Należy pamiętać, wersja określonego środowiska uruchomieniowego i żądanej wersji głównej. W poniższym przykładzie funkcje\_rozszerzenia\_wersji aplikacji został ustawiony na `~1`.
 
   ![Wybierz ustawienia aplikacji — funkcja](./media/functions-versions/function-app-view-version.png)

## <a name="target-the-version-20-runtime"></a>Docelowy środowiska wykonawczego w wersji 2.0

>[!IMPORTANT]   
> Środowisko uruchomieniowe Functions Azure 2.0 jest w wersji zapoznawczej i obecnie nie wszystkie funkcje usługi Azure Functions są obsługiwane. Aby uzyskać więcej informacji, zobacz [różnice między środowiska uruchomieniowego 1.x i 2.x](#differences-between-runtime-1x-and-2x) we wcześniejszej części tego artykułu.

Aplikacja funkcji można przenieść do podglądu środowiska wykonawczego w wersji 2.0 w portalu Azure. W **funkcji ustawienia aplikacji** , wybierz pozycję **beta** w obszarze **wersja środowiska uruchomieniowego**.  

![Wybierz ustawienia aplikacji — funkcja](./media/functions-versions/function-app-view-version.png)

To ustawienie jest odpowiednikiem ustawienia `FUNCTIONS_EXTENSION_VERSION` ustawienie aplikacji, aby `beta`. Wybierz **~ 1** przycisk, aby powrócić do bieżącego publicznie obsługiwanych wersji głównej. Aby zaktualizować to ustawienie aplikacji umożliwia także wiersza polecenia platformy Azure. 

## <a name="target-a-version-using-the-portal"></a>Docelowa wersja przy użyciu portalu

Jeśli potrzebne jest docelowa wersja niż bieżąca wersja główna lub w wersji 2.0, należy ustawić `FUNCTIONS_EXTENSION_VERSION` ustawienia aplikacji.

1. W [portalu Azure](https://portal.azure.com), przejdź do aplikacji funkcji i w obszarze **skonfigurować funkcje**, wybierz **ustawienia aplikacji**.

    ![Wybierz ustawienia aplikacji — funkcja](./media/functions-versions/add-update-app-setting1a.png)

2. W **ustawienia aplikacji** karcie, Znajdź `FUNCTIONS_EXTENSION_VERSION` ustawienie i zmień wartość na prawidłową wersję środowiska uruchomieniowego 1.x lub `beta` w wersji 2.0. 

    ![Ustawienia aplikacji — funkcja](./media/functions-versions/add-update-app-setting2.png)

3. Kliknij przycisk **zapisać** można zapisać ustawienia aplikacji. 

## <a name="target-a-version-using-azure-cli"></a>Docelowa wersja przy użyciu wiersza polecenia platformy Azure

 Można również ustawić `FUNCTIONS_EXTENSION_VERSION` z wiersza polecenia platformy Azure. Przy użyciu wiersza polecenia platformy Azure, zaktualizuj ustawienia aplikacji w aplikacji funkcji z [az functionapp config appsettings zestaw](/cli/azure/functionapp/config/appsettings#az_functionapp_config_appsettings_set) polecenia.

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group <my_resource_group> \
--settings FUNCTIONS_EXTENSION_VERSION=<version>
```
W tym kodzie Zamień `<function_app>` z nazwą funkcji aplikacji. Zastąp również `<my_resource_group>` z nazwą grupy zasobów dla aplikacji funkcja. Zastąp `<version>` z prawidłową wersją środowiska uruchomieniowego 1.x lub `beta` w wersji 2.0. 

Możesz uruchomić to polecenie z [powłoki chmury Azure](../cloud-shell/overview.md) , wybierając **wypróbuj** w poprzednim przykładzie kodu. Można również użyć [lokalnie interfejsu wiersza polecenia Azure](/cli/azure/install-azure-cli) do wykonywania tego polecenia po wykonaniu [logowania az](/cli/azure#az_login) do logowania.

## <a name="next-steps"></a>Kolejne kroki

> [!div class="nextstepaction"]
> [Do wykonywania 2.0 w środowisku projektowania lokalnego](functions-run-local.md)

> [!div class="nextstepaction"]
> [Zobacz informacje o wersji dla wersji środowiska uruchomieniowego](https://github.com/Azure/azure-webjobs-sdk-script/releases)