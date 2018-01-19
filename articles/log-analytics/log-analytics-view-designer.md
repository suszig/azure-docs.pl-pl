---
title: "Tworzenie widoków w celu analizowania danych w Azure Log Analytics | Dokumentacja firmy Microsoft"
description: "Widok projektanta w analizy dzienników umożliwia tworzenie niestandardowych widoków, które są wyświetlane w portalu Azure i zawierają różne wizualizacje danych w obszarze roboczym analizy dzienników. Ten artykuł zawiera omówienie projektanta widoków i procedur tworzenia i edytowania widoków niestandardowych."
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
ms.date: 01/18/2018
ms.author: bwren
ms.openlocfilehash: a84f40503c1b9778c496461ebbf6864f99bd1c4b
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="use-view-designer-to-create-custom-views-in-log-analytics"></a>Projektant widoków umożliwia tworzenie niestandardowych widoków w analizy dzienników
Projektant widoków w [analizy dzienników](log-analytics-overview.md) umożliwia tworzenie niestandardowych widoków w portalu Azure, która zawiera różne wizualizacje danych w obszarze roboczym analizy dzienników. Ten artykuł zawiera omówienie projektanta widoków i procedur tworzenia i edytowania widoków niestandardowych.

Inne artykuły, które są dostępne dla projektanta widoku są następujące:

* [Kafelek odwołanie](log-analytics-view-designer-tiles.md) — odwołanie do ustawienia dla każdego z dostępnych do użycia w niestandardowych widoków Kafelki.
* [Odwołanie do części wizualizacji](log-analytics-view-designer-parts.md) — odwołanie do ustawienia dla każdego z dostępnych do użycia w niestandardowych widoków Kafelki.

>[!NOTE]
> Jeśli obszaru roboczego został uaktualniony do [języka zapytań nowe analizy dzienników](log-analytics-log-search-upgrade.md), a następnie zapytania we wszystkich widokach musi być napisana w [nowy język kwerendy](https://go.microsoft.com/fwlink/?linkid=856078).  Wszystkie widoki, które zostały utworzone przed obszaru roboczego został uaktualniony będzie automtically przekonwertować.

## <a name="concepts"></a>Pojęcia
Widoki są wyświetlane na **omówienie** strony obszaru roboczego analizy dzienników w portalu Azure.  Kafelek każdy widok niestandardowy pojawi się alfabetycznie z Kafelki na tym samym obszarze roboczym zainstalowane rozwiązania.

![Strona przeglądu](media/log-analytics-view-designer/overview-page.png)

Widoki utworzone za pomocą projektanta widoku zawierają elementy w tabeli poniżej.

| Część | Opis |
|:--- |:--- |
| Kafelek |Wyświetlane na stronie Omówienie obszaru roboczego analizy dzienników.  Obejmuje visual podsumowywania informacji zawartych w widoku niestandardowym.  Różne typy kafelka zapewniają różne wizualizacje rekordów.  Kliknij Kafelek, aby otworzyć widok niestandardowy. |
| Widok niestandardowy |Wyświetlane, gdy użytkownik kliknie na kafelku.  Zawiera co najmniej jeden części wizualizacji. |
| Części wizualizacji |Wizualizacja danych w obszarze roboczym analizy dzienników oparte na co najmniej jednym [dziennika wyszukiwania](log-analytics-log-searches.md).  Większość elementów będzie zawierać nagłówek, który zapewnia wysokiego poziomu wizualizacji i lista wyników top.  Typy różnych części zapewniają różne wizualizacje rekordów w obszarze roboczym analizy dzienników.  Kliknij elementy w części do wyszukiwania dziennika, zapewniając szczegółowe dane. |


## <a name="work-with-an-existing-view"></a>Praca z istniejącego widoku
Po otwarciu widoku, który został utworzony z projektanta widoków jej menu z opcjami w poniższej tabeli.

![Menu — omówienie](media/log-analytics-view-designer/overview-menu.png)


| Opcja | Opis |
|:--|:--|
| Odświeżanie   | Odśwież widok przy użyciu najnowszych danych. | 
| Analiza | Otwórz [portal analityka zaawansowane](log-analytics-log-search-portals.md#advanced-analytics-portal) do analizowania danych z dziennika wyszukiwania. () log-Analytics-log-Search-portals.MD#Advanced-Analytics-Portal). |
| Filtr    | Ustaw filtr czasu dla danych uwzględnione w widoku. |
| Edytuj      | Otwórz widok w Projektancie widoku do edycji jej zawartości i konfiguracji.   |
| Klonuj     | Utwórz nowy widok, a następnie otwórz go w Projektancie widoku.  Nowy widok ma taką samą nazwę jak oryginał z "Kopiuj" dołączany na końcu. |


## <a name="create-a-new-view"></a>Utwórz nowy widok
Utwórz nowy widok w **Widok projektanta** przez kliknięcie kafelka Projektant widoków na stronie Omówienie obszaru roboczego analizy dzienników w portalu Azure.

![Widok projektanta kafelka](media/log-analytics-view-designer/view-designer-tile.png)


## <a name="working-with-view-designer"></a>Praca z projektanta widoków
Czy podczas tworzenia nowego widoku lub edytowanie istniejących będzie działać z projektanta widoków.  

Projektant widoków ma trzy okienka.  **Projekt** okienko zawiera widok niestandardowy, który podczas tworzenia lub edycji.  Dodaj Kafelki i składniki Report Part z **kontroli** okienko, aby **projekt** okienka.  **Właściwości** okienku zostaną wyświetlone właściwości kafelka lub wybranej części.

![Projektant widoków](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Konfigurowanie widoku kafelków
Widok niestandardowy może mieć tylko jednego kafelka.  Wybierz **Kafelek** karcie **kontroli** okienko, aby wyświetlić bieżący Kafelek lub wybierz alternatywny.  **Właściwości** okienku zostaną wyświetlone właściwości dla bieżącego fragmentu.  Skonfiguruj właściwości kafelka zgodnie ze szczegółowych informacji w [Kafelek odwołania](log-analytics-view-designer-tiles.md) i kliknij przycisk **Zastosuj** można zapisać zmian.

### <a name="configure-visualization-parts"></a>Skonfiguruj części wizualizacji
Widok może zawierać dowolną liczbę części wizualizacji.  Wybierz **widoku** kartę, a następnie do części wizualizacji, aby dodać do widoku.  **Właściwości** okienku zostaną wyświetlone właściwości wybranej części.  Skonfiguruj właściwości widoku zgodnie ze szczegółowych informacji w [odwołanie do części wizualizacji](log-analytics-view-designer-parts.md) i kliknij przycisk **Zastosuj** można zapisać zmian.

Widoki mieć tylko jeden wiersz części wizualizacji.  Rozmieszczanie istniejące składniki w widoku, klikając i przeciągając je do nowej lokalizacji.

Part wizualizacji można usunąć z widoku, klikając **X** przycisk w prawym górnym rogu części.


### <a name="menu-options"></a>Opcje menu
Podczas pracy z widoku w trybie edycji, masz opcji menu w poniższej tabeli.

![Menu Edycja](media/log-analytics-view-designer/edit-menu.png)

| Opcja | Opis |
|:--|:--|
| Zapisz        | Zapisz zmiany i zamknąć widok. |
| Cancel      | Odrzucić zmiany i zamknąć widok. |
| Usuń widok | Usuń widok. |
| Eksportowanie      | Widok, aby wyeksportować [szablonu usługi Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) , który można zaimportować do innego obszaru roboczego.  Nazwa pliku będzie nazwa widoku z rozszerzeniem *omsview*. |
| Import      | Importuj *omsview* pliku, który został wyeksportowany z innego obszaru roboczego.  Spowoduje to zastąpienie konfigurację istniejącego widoku. |
| Klonuj       | Utwórz nowy widok, a następnie otwórz go w Projektancie widoku.  Nowy widok ma taką samą nazwę jak oryginał z "Kopiuj" dołączany na końcu. |
| Publikowanie     | Eksportuj do pliku JSON, który można wstawiać do widoku [rozwiązania Mangagement](../operations-management-suite/operations-management-suite-solutions-resources-views.md).  Nazwa pliku będzie nazwa widoku z rozszerzeniem *json*. Drugi plik jest tworzony z rozszerzeniem *resjson* zawierającą wartości zasoby zdefiniowane w pliku JSON.

## <a name="next-steps"></a>Kolejne kroki
* Dodaj [Kafelki](log-analytics-view-designer-tiles.md) do widoku niestandardowym.
* Dodaj [części wizualizacji](log-analytics-view-designer-parts.md) do widoku niestandardowym.
