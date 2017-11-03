---
title: "Zarządzanie aplikacjami logikę w programie Visual Studio — usługi Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Zarządzanie logiki aplikacji i innych zasobów platformy Azure z programu Visual Studio Cloud Explorer"
author: klam
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 12/19/2016
ms.author: LADocs; klam
ms.openlocfilehash: a5bf24de1a7a2b6d4c1ae6416c95d83ef7506da3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-your-logic-apps-with-visual-studio-cloud-explorer"></a>Zarządzanie aplikacjami logiki za pomocą programu Visual Studio Cloud Explorer

Mimo że [portalu Azure](https://portal.azure.com/) to dobry sposób projektowania i zarządzania usługi Azure Logic Apps oferuje, możesz użyć programu Visual Studio Cloud Explorer zarządzania wielu zasobów platformy Azure, w tym aplikacji logiki. Visual Studio Cloud Explorer umożliwia przeglądanie, zarządzanie, edytowanie i pobierania opublikowane aplikacje logiki. Zadania zarządzania obejmują Włącz, wyłącz i uruchom wyświetlanie historii. 

Można było uzyskać dostęp i zarządzać aplikacji logiki w programie Visual Studio, zainstalować i skonfigurować te narzędzia Visual Studio dla usługi Azure Logic Apps. 

## <a name="prerequisites"></a>Wymagania wstępne

* [Visual Studio 2015 lub Visual Studio 2017 r.](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* [Najnowszy zestaw Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 lub nowszej)
* [Visual Studio Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015)
* Dostęp do sieci web, gdy przy użyciu narzędzia Projektant osadzonych

## <a name="install-visual-studio-tools-for-logic-apps"></a>Instalowanie narzędzi Visual Studio tools for Logic Apps

Po zainstalowaniu wymagań wstępnych, Pobierz i zainstaluj narzędzia aplikacje logiki platformy Azure dla programu Visual Studio.

1. Otwórz program Visual Studio. Na **narzędzia** menu, wybierz opcję **rozszerzenia i aktualizacje**.
2. Rozwiń węzeł **Online** kategorii, Wyszukaj w trybie online w galerii programu Visual Studio.
3. Wyszukaj **Logic Apps** do momentu znalezienia **Azure Logic Apps Tools for Visual Studio**.
4. Kliknij, aby pobrać i zainstalować rozszerzenie **Pobierz**.
5. Po zakończeniu instalacji uruchom ponownie program Visual Studio.

> [!NOTE]
> Aby bezpośrednio pobrać narzędzia aplikacje logiki platformy Azure dla programu Visual Studio, przejdź do [Visual Studio Marketplace](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9).

## <a name="browse-for-logic-apps-in-cloud-explorer"></a>Przeglądaj w poszukiwaniu aplikacji logiki w Eksploratorze chmury

1.  Aby otworzyć Eksplorator chmury w **widoku** menu, wybierz **Eksplorator chmury**.
2.  Przeglądaj w poszukiwaniu aplikacji logiki, grupy zasobów lub typ zasobu. 

    * Jeśli możesz przeglądać według typów zasobów, wybierz subskrypcję platformy Azure, rozwiń **Logic Apps** sekcji, a następnie wybierz aplikację logiki. 
    * Podczas przeglądania według grupy zasobów, rozwiń grupę zasobów, która zawiera aplikację logiki, a następnie wybierz aplikację logiki.

    Aby wyświetlić polecenia aplikacji logiki, kliknij prawym przyciskiem myszy aplikację logiki, albo w dolnej części Eksplorator chmury, wybierz jedną z **akcje** menu.

    ![Przeglądaj w poszukiwaniu aplikacji logiki](./media/logic-apps-manage-from-vs/browse.png)

## <a name="edit-your-logic-app-with-logic-apps-designer"></a>Edytuj w Projektancie aplikacji logiki aplikacji logiki

W Eksploratorze chmury można otworzyć aplikacji logiki aktualnie wdrożonych w tej samej projektancie używanego w portalu Azure. 

* Aby edytować aplikację logiki w Eksploratorze chmury, kliknij prawym przyciskiem myszy aplikację logiki, a następnie wybierz **otwarty przy użyciu edytora aplikacji logiki**. 

* Aby opublikować aktualizacje w chmurze, wybrać **publikowania**. 

* Aby uruchomić nowy przebieg, wybierz **uruchomić wyzwalacz**.

![Projektant aplikacji logiki](./media/logic-apps-manage-from-vs/designer.png)

Przy użyciu projektanta, możesz również **Pobierz** aplikacji logiki. Ta akcja parameterizes definicję aplikacji logiki i automatycznie definicji jest zapisywany jako szablon wdrożenia usługi Azure Resource Manager. Ten szablon wdrożenia można dodać do projektu grupy zasobów platformy Azure.

## <a name="browse-your-logic-app-run-history"></a>Przeglądaj Historia uruchomień aplikacji logiki

Aby wyświetlić historię wykonywania aplikacji logiki, kliknij prawym przyciskiem myszy aplikację logiki i wybierz **Historia uruchomień Otwórz**. Aby zmienić kolejność wykonywania historii oparty na żadnym z właściwości wyświetlane, wybierz nagłówek kolumny.

![Historia uruchomień](media/logic-apps-manage-from-vs/runs.png)

Można wyświetlić historię uruchomień na potrzeby wystąpienia, można przejrzeć wyniki uruchomienia, łącznie z wejściami i wyjściami z każdym kroku, kliknij dwukrotnie jedno wystąpienie wykonywania.

![Uruchom wyników historii, wejścia i wyjścia z kroków](./media/logic-apps-manage-from-vs/history.png)

## <a name="next-steps"></a>Następne kroki

* [Tworzenie pierwszej aplikacji logiki](logic-apps-create-a-logic-app.md)
* [Projektowanie, tworzenie i wdrażanie aplikacji logiki w programie Visual Studio](logic-apps-deploy-from-vs.md)
* [Wyświetlanie typowych przykładów i scenariuszy](logic-apps-examples-and-scenarios.md)
* [Wideo: Automatyzować procesy biznesowe z usługi Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/T694)
* [Wideo: Integrować systemy z usługą Azure Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
