---
title: "Tworzenie, tworzenie i wdrażanie aplikacji logiki w Visual Studio — usługi Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Tworzenie projektów programu Visual Studio, projektowanie, tworzenie i wdrażanie usługi Azure Logic Apps."
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e484e5ce-77e9-4fa9-bcbe-f851b4eb42a6
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 2/14/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: e7f5cf483d22e4c60dedbe5176ceb0bc8b2b6e66
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="design-build-and-deploy-azure-logic-apps-in-visual-studio"></a>Projektowanie, tworzenie i wdrażanie usługi Azure Logic Apps w programie Visual Studio

Mimo że [portalu Azure](https://portal.azure.com/) oferuje to dobry sposób umożliwiające tworzenie i zarządzanie nimi Azure Logic Apps, można użyć programu Visual Studio umożliwiające projektowanie, tworzenie i wdrażanie aplikacji logiki. Program Visual Studio udostępnia zaawansowane narzędzia, takie jak dla projektanta aplikacji logiki do tworzenia aplikacji logiki, konfigurować szablony wdrażania i automatyzacji i wdrażania z dowolnym środowiskiem. 

Aby rozpocząć pracę z usługą Azure Logic Apps, Dowiedz się [tworzenie pierwszej aplikacji logiki w portalu Azure](logic-apps-create-a-logic-app.md).

## <a name="installation-steps"></a>Kroki instalacji

Instalowanie i Konfigurowanie narzędzi Visual Studio tools for Azure Logic Apps, wykonaj następujące kroki.

### <a name="prerequisites"></a>Wymagania wstępne

* [Visual Studio 2017](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx) lub programu Visual Studio 2015
* [Najnowszy zestaw Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 lub nowszej)
* [Azure PowerShell](https://github.com/Azure/azure-powershell#installation)
* Dostęp do sieci web, gdy przy użyciu narzędzia Projektant osadzonych

### <a name="install-visual-studio-tools-for-azure-logic-apps"></a>Instalowanie narzędzi Visual Studio tools for Azure Logic Apps

Po zainstalowaniu wymagań wstępnych:

1. Otwórz program Visual Studio. Na **narzędzia** menu, wybierz opcję **rozszerzenia i aktualizacje**.
2. Rozwiń węzeł **Online** kategorii można wyszukiwać w tryb online.
3. Wyszukaj **Logic Apps** do momentu znalezienia **Azure Logic Apps Tools for Visual Studio**.
4. Kliknij, aby pobrać i zainstalować rozszerzenie **Pobierz**.
5. Po zakończeniu instalacji uruchom ponownie program Visual Studio.

> [!NOTE]
> Możesz również pobrać [narzędzia aplikacje logiki platformy Azure dla programu Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) i [narzędzia aplikacje logiki platformy Azure dla programu Visual Studio 2015](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio) bezpośrednio z programu Visual Studio Marketplace.

Po zakończeniu instalacji, używając projektu grupy zasobów platformy Azure za pomocą projektanta aplikacji logiki.

## <a name="create-your-project"></a>Tworzenie projektu

1. Na **pliku** menu, przejdź do **nowy**i wybierz **projektu**. Lub aby dodać projektu do istniejącego rozwiązania, przejdź do **Dodaj**i wybierz **nowy projekt**.

    ![Menu Plik](./media/logic-apps-deploy-from-vs/filemenu.png)

2. W **nowy projekt** oknie Znajdź **chmury**i wybierz **grupy zasobów platformy Azure**. Nazwij swój projekt, a następnie kliknij przycisk **OK**.

    ![Dodaj nowy projekt](./media/logic-apps-deploy-from-vs/addnewproject.png)

3. Wybierz **aplikacji logiki** szablonu, który tworzy logiki pusty szablon wdrożenia aplikacji można użyć. Po wybraniu szablonu, kliknij przycisk **OK**.

    ![Wybierz szablon aplikacji logiki](./media/logic-apps-deploy-from-vs/selectazuretemplate1.png)

    Teraz dodane projektu aplikacji logiki do rozwiązania. 
    W Eksploratorze rozwiązań powinien pojawić się pliku wdrożenia.

    ![Plik wdrożenia](./media/logic-apps-deploy-from-vs/deployment.png)

## <a name="create-your-logic-app-with-logic-app-designer"></a>Tworzenie aplikacji logiki z projektanta aplikacji logiki

Jeśli projekt grupy zasobów platformy Azure, który zawiera aplikację logiki, możesz otworzyć projektanta aplikacji logiki w programie Visual Studio, aby utworzyć przepływ pracy. 

> [!NOTE]
> Projektant wymaga połączenia internetowego na łączniki zapytania dla danych i dostępnych właściwości. Na przykład jeśli używany jest łącznik usługi Dynamics CRM Online, wysyła zapytanie wystąpienia CRM, aby wyświetlić dostępne niestandardowe i domyślne właściwości projektanta.

1. Kliknij prawym przyciskiem myszy użytkownika `<template>.json` pliku, a następnie wybierz **Otwórz w Projektancie aplikacji logiki**. (`Ctrl+L`)

2. Wybierz subskrypcję platformy Azure, grupy zasobów i lokalizacji szablonu wdrożenia.

    > [!NOTE]
    > Projektowanie aplikacji logiki tworzy połączenie z interfejsem API zasobów zapytania dla właściwości podczas projektowania. Visual Studio będzie korzystać z wybranej grupy zasobów można utworzyć te połączenia w czasie projektowania. Aby wyświetlić lub zmienić dowolne połączenia interfejsu API, przejdź do portalu Azure, a następnie przejdź do **połączenia interfejsu API**.

    ![Selektor subskrypcji](./media/logic-apps-deploy-from-vs/designer_picker.png)

    Projektant korzysta z definicją w `<template>.json` pliku do renderowania.

4. Tworzenie i projektowanie aplikacji logiki. Szablon wdrożenia zostanie zaktualizowany z wprowadzonymi zmianami.

    ![Projektant aplikacji logiki w programie Visual Studio](./media/logic-apps-deploy-from-vs/designer_in_vs.png)

Dodaje programu Visual Studio `Microsoft.Web/connections` zasobów do pliku zasobów dla połączeń aplikację logiki musi działać. Te właściwości połączenia można ustawić podczas wdrażania i zarządzania po wdrożeniu w **połączenia interfejsu API** w portalu Azure.

### <a name="switch-to-json-code-view"></a>Przełącz do widoku kodu JSON

Aby wyświetlić reprezentacja JSON aplikacji logiki, wybierz **widoku kodu** u dołu projektanta.

Aby przełączyć się do wszystkich zasobów JSON, kliknij prawym przyciskiem myszy `<template>.json` pliku, a następnie wybierz **Otwórz**.

### <a name="add-references-for-dependent-resources-to-visual-studio-deployment-templates"></a>Dodaj odwołania do zasobów zależnych w szablonach wdrożenia programu Visual Studio

Jeśli chcesz aplikację logiki, aby odwołać zasoby zależne, można użyć [funkcje szablonu usługi Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-functions) w szablonie wdrożenia aplikacji logiki. Na przykład można aplikację logiki, aby odwołać konto funkcji platformy Azure lub integracji, które chcesz wdrożyć obok aplikacji logiki. Wykonaj te wskazówki dotyczące sposobu użycia parametrów w szablonie wdrożenia, aby w Projektancie aplikacji logiki renderuje poprawnie. 

Można używać parametrów aplikacji logiki w rodzaju wyzwalacze i akcje:

*   Podrzędny przepływ pracy
*   Funkcja aplikacji
*   Wywołanie APIM
*   Adres URL wykonywania połączenia interfejsu API
*   Ścieżka połączenia interfejsu API

I możesz użyć funkcji szablonu, takie jak parametry, zmienne, resourceId, concat itp. Na przykład Oto jak można zastąpić identyfikator zasobu funkcji platformy Azure:

```
"parameters":{
    "functionName": {
        "type":"string",
        "minLength":1,
        "defaultValue":"<FunctionName>"
    }
},
```

I użycia parametrów:

```
"MyFunction": {
    "type": "Function",
    "inputs": {
        "body":{},
        "function":{
            "id":"[resourceid('Microsoft.Web/sites/functions','functionApp',parameters('functionName'))]"
        }
    },
    "runAfter":{}
}
```
Innym przykładem mogą parametryzacja operację wysyłania komunikatu magistrali usług:

```
"Send_message": {
    "type": "ApiConnection",
        "inputs": {
            "host": {
                "connection": {
                    "name": "@parameters('$connections')['servicebus']['connectionId']"
                }
            },
            "method": "post",
            "path": "[concat('/@{encodeURIComponent(''', parameters('queueuname'), ''')}/messages')]",
            "body": {
                "ContentData": "@{base64(triggerBody())}"
            },
            "queries": {
                "systemProperties": "None"
            }
        },
        "runAfter": {}
    }
```
> [!NOTE] 
> host.runtimeUrl jest opcjonalny i może być usunięty z szablonu, jeśli jest obecny.
> 


> [!NOTE] 
> Dla projektanta aplikacji logiki do działa w przypadku użycia parametrów należy podać wartości domyślnych, na przykład:
> 
> ```
> "parameters": {
>     "IntegrationAccount": {
>     "type":"string",
>     "minLength":1,
>     "defaultValue":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Logic/integrationAccounts/<integrationAccountName>"
>     }
> },
> ```

### <a name="save-your-logic-app"></a>Zapisywanie aplikacji logiki

Aby zapisać aplikację logiki w dowolnym momencie, przejdź do **pliku** > **zapisać**. (`Ctrl+S`) 

Gdy aplikacja logiki ma błędy podczas zapisywania aplikacji, są wyświetlane w programie Visual Studio **dane wyjściowe** okna.

## <a name="deploy-your-logic-app-from-visual-studio"></a>Wdrażanie aplikacji logiki w programie Visual Studio

Po skonfigurowaniu aplikacji, można wdrożyć bezpośrednio z programu Visual Studio w zaledwie kilku krokach. 

1. W Eksploratorze rozwiązań kliknij projekt prawym przyciskiem myszy, a następnie przejdź do **Wdróż** > **nowego wdrożenia...**

    ![Nowe wdrożenie](./media/logic-apps-deploy-from-vs/newdeployment.png)

2. Po wyświetleniu monitu zaloguj się do subskrypcji platformy Azure. 

3. Teraz musisz wybrać szczegóły dla grupy zasobów, w której chcesz wdrożyć aplikację logiki. Gdy wszystko będzie gotowe, wybierz **Wdróż**.

    > [!NOTE]
    > Upewnij się, że wybrano właściwy plik szablonu i parametry dla grupy zasobów. Na przykład jeśli chcesz wdrożyć w środowisku produkcyjnym, wybierz plik parametrów produkcji.

    ![Wdrażanie w grupie zasobów](./media/logic-apps-deploy-from-vs/deploytoresourcegroup.png)

    Stan wdrażania zostanie wyświetlony w **dane wyjściowe** okna. 
    Być może trzeba wybrać **inicjowania obsługi usługi Azure** w **Pokaż dane wyjściowe z** listy.

    ![Dane wyjściowe stanu wdrożenia](./media/logic-apps-deploy-from-vs/output.png)

W przyszłości można edytować aplikację logiki w kontroli źródła i wdrażanie nowych wersji za pomocą programu Visual Studio.

> [!NOTE]
> Bezpośrednio w przypadku zmiany definicji w portalu Azure, zmiany te zostaną zastąpione podczas wdrażania w programie Visual Studio następnym. 

## <a name="add-your-logic-app-to-an-existing-resource-group-project"></a>Dodaj do istniejącego projektu grupy zasobów aplikacji logiki

Jeśli masz istniejący projekt grupy zasobów aplikacji logiki można dodać do tego projektu w okna konspekt pliku JSON. Można również dodać inną aplikację logiki równolegle z aplikacji, która wcześniej została utworzona.

1. Otwórz plik `<template>.json`.

2. Aby otworzyć okno konspekt pliku JSON, przejdź do **widoku** > **inne okna** > **konspekt pliku JSON**.

3. Aby dodać zasobu do pliku szablonu, kliknij przycisk **dodawania zasobów** u góry okna konspekt pliku JSON. W oknie Konspekt pliku JSON, kliknij prawym przyciskiem myszy **zasobów**i wybierz **Dodaj nowy zasób**.

    ![Okna konspekt pliku JSON](./media/logic-apps-deploy-from-vs/jsonoutline.png)
    
4. W **dodawania zasobów** okno dialogowe, Znajdź i wybierz **aplikacji logiki**. Określ nazwę aplikacji logiki, a następnie wybierz pozycję **Dodaj**.

    ![Dodaj zasób](./media/logic-apps-deploy-from-vs/addresource.png)

## <a name="next-steps"></a>Następne kroki

* [Zarządzanie logiki aplikacji za pomocą programu Visual Studio Cloud Explorer](logic-apps-manage-from-vs.md)
* [Wyświetlanie typowych przykładów i scenariuszy](logic-apps-examples-and-scenarios.md)
* [Dowiedz się, jak automatyzować procesy biznesowe z usługi Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694)
* [Dowiedz się, jak integrować systemy z usługą Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
