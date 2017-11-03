---
title: "Utwórz widoki do analizowania danych w OMS Log Analytics | Dokumentacja firmy Microsoft"
description: "Widok projektanta w analizy dzienników umożliwia tworzenie niestandardowych widoków, które są wyświetlane w portalu OMS i na platformie Azure i zawierać różne wizualizacje danych w repozytorium OMS. Ten artykuł zawiera omówienie projektanta widoków i procedur tworzenia i edytowania widoków niestandardowych."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: e3c463d749dc4179df58286b9bb75584880a6bc6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-view-designer-to-create-custom-views-in-log-analytics"></a>Projektant widoków umożliwia tworzenie niestandardowych widoków w analizy dzienników
Projektant widoków w [analizy dzienników](log-analytics-overview.md) umożliwia tworzenie niestandardowych widoków w konsoli OMS, która zawiera różne wizualizacje danych w repozytorium OMS. Ten artykuł zawiera omówienie projektanta widoków i procedur tworzenia i edytowania widoków niestandardowych.

Inne artykuły, które są dostępne dla projektanta widoku są następujące:

* [Kafelek odwołanie](log-analytics-view-designer-tiles.md) — odwołanie do ustawienia dla każdego z dostępnych do użycia w niestandardowych widoków Kafelki.
* [Odwołanie do części wizualizacji](log-analytics-view-designer-parts.md) — odwołanie do ustawienia dla każdego z dostępnych do użycia w niestandardowych widoków Kafelki.

>[!NOTE]
> Jeśli obszaru roboczego został uaktualniony do [języka zapytań nowe analizy dzienników](log-analytics-log-search-upgrade.md), a następnie zapytania we wszystkich widokach musi być napisana w [nowy język kwerendy](https://go.microsoft.com/fwlink/?linkid=856078).  Wszystkie widoki, które zostały utworzone przed obszaru roboczego został uaktualniony będzie automtically przekonwertować.

## <a name="concepts"></a>Pojęcia
Widoki utworzone za pomocą projektanta widoku zawierają elementy w tabeli poniżej.

| Część | Opis |
|:--- |:--- |
| Kafelek |Wyświetlane na głównym pulpicie nawigacyjnym omówienie analizy dziennika.  Obejmuje visual podsumowywania informacji zawartych w widoku niestandardowym.  Różne typy kafelka zapewniają różne wizualizacje rekordów w repozytorium OMS.  Kliknij Kafelek, aby otworzyć widok niestandardowy. |
| Widok niestandardowy |Wyświetlane, gdy użytkownik kliknie na kafelku.  Zawiera co najmniej jeden części wizualizacji. |
| Części wizualizacji |Wizualizacja danych w repozytorium OMS oparte na co najmniej jeden [dziennika wyszukiwania](log-analytics-log-searches.md).  Większość elementów będzie zawierać nagłówek, który zapewnia wysokiego poziomu wizualizacji i lista wyników top.  Typy różnych części zapewniają różne wizualizacje rekordów w repozytorium OMS.  Kliknij elementy w części do wyszukiwania dziennika, zapewniając szczegółowe dane. |

![Omówienie projektanta widoku](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-to-your-workspace"></a>Dodaj widok projektanta do swojego obszaru roboczego
Gdy widok projektanta jest w wersji zapoznawczej, należy dodać go do swojego obszaru roboczego, wybierając **funkcje w wersji zapoznawczej** w **ustawienia** części portalu OMS.

![Włącz podgląd](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Tworzenie i edytowanie widoków
### <a name="create-a-new-view"></a>Utwórz nowy widok
Otwórz nowy widok w **Widok projektanta** przez kliknięcie kafelka Projektant widoków na głównym pulpicie nawigacyjnym OMS.

![Widok projektanta kafelka](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Edytuj istniejący widok
Aby dokonać edycji istniejącego widoku w Projektancie widok, otwórz widok, klikając jej kafelka na głównym pulpicie nawigacyjnym OMS.  Następnie kliknij przycisk **Edytuj** przycisk, aby otworzyć widok w Projektancie widoku.

![Edytowanie widoku](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Klonowanie istniejącego widoku
Podczas klonowania widoku, tworzy nowy widok i otwarcie go w Projektancie widoku.  Nowy widok będzie mieć taką samą nazwę jak oryginał z "Kopiuj" dołączany na końcu.  Klonowanie widok, otwórz istniejący widok, klikając jej kafelka na głównym pulpicie nawigacyjnym OMS.  Następnie kliknij przycisk **klonowania** przycisk, aby otworzyć widok w Projektancie widoku.

![Klonowanie widoku](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Usuwanie istniejącego widoku
Aby usunąć istniejący widok, otwórz widok, klikając jej kafelka na głównym pulpicie nawigacyjnym OMS.  Następnie kliknij przycisk **Edytuj** przycisk, aby otworzyć widok w Projektancie widoku, a następnie kliknij przycisk **Usuń widok**.

![Usuwanie widoku](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Eksportuj istniejącego widoku
Widok można wyeksportować do pliku JSON, który można zaimportować do innego obszaru roboczego lub użyć w [szablonu usługi Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).  Aby wyeksportować istniejący widok, otwórz widok, klikając jej kafelka na głównym pulpicie nawigacyjnym OMS.  Następnie kliknij przycisk **wyeksportować** przycisk, aby utworzyć plik w folderze pobierania w przeglądarce.  Nazwa pliku będzie nazwa widoku z rozszerzeniem *omsview*.

![Eksportowanie widoku](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Importuj istniejący widok
Możesz zaimportować *omsview* pliku, który został wyeksportowany z innej grupy zarządzania.  Aby zaimportować istniejący widok, należy najpierw utworzyć nowy widok.  Następnie kliknij przycisk **importu** i wybrać *omsview* pliku.  W pliku konfiguracji zostaną skopiowane do istniejącego widoku.

![Eksportowanie widoku](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Praca z projektanta widoków
Projektant widoków ma trzy okienka.  **Projekt** okienko reprezentuje widok niestandardowy.  Po dodaniu Kafelki i składniki Report Part z **kontroli** okienko, aby **projekt** są one dodawane do widoku okienka.  **Właściwości** okienku zostaną wyświetlone właściwości kafelka lub wybranej części.

![Projektant widoków](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Konfigurowanie widoku kafelków
Widok niestandardowy może mieć tylko jednego kafelka.  Wybierz **Kafelek** karcie **kontroli** okienko, aby wyświetlić bieżący Kafelek lub wybierz alternatywny.  **Właściwości** okienku zostaną wyświetlone właściwości dla bieżącego fragmentu.  Skonfiguruj właściwości kafelka zgodnie ze szczegółowych informacji w [Kafelek odwołania](log-analytics-view-designer-tiles.md) i kliknij przycisk **Zastosuj** można zapisać zmian.

### <a name="configure-visualization-parts"></a>Skonfiguruj części wizualizacji
Widok może zawierać dowolną liczbę części wizualizacji.  Wybierz **widoku** kartę, a następnie do części wizualizacji, aby dodać do widoku.  **Właściwości** okienku zostaną wyświetlone właściwości wybranej części.  Skonfiguruj właściwości widoku zgodnie ze szczegółowych informacji w [odwołanie do części wizualizacji](log-analytics-view-designer-parts.md) i kliknij przycisk **Zastosuj** można zapisać zmian.

### <a name="delete-a-visualization-part"></a>Usunąć części wizualizacji
Part wizualizacji można usunąć z widoku, klikając **X** przycisk w prawym górnym rogu części.

### <a name="rearrange-visualization-parts"></a>Rozmieszczanie części wizualizacji
Widoki mieć tylko jeden wiersz części wizualizacji.  Rozmieszczanie istniejące składniki w widoku, klikając i przeciągając je do nowej lokalizacji.

## <a name="next-steps"></a>Następne kroki
* Dodaj [Kafelki](log-analytics-view-designer-tiles.md) do widoku niestandardowym.
* Dodaj [części wizualizacji](log-analytics-view-designer-parts.md) do widoku niestandardowym.
