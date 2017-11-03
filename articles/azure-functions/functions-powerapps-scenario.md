---
title: "Wywoływanie funkcji z rozwiązania PowerApps | Dokumentacja firmy Microsoft"
description: "Tworzenie niestandardowego łącznika, a następnie wywołaj funkcję za pomocą tego łącznika."
services: functions
keywords: "aplikacje w chmurze, w chmurze usługi, w rozwiązaniu PowerApps, procesów biznesowych, aplikacja biznesowa"
documentationcenter: 
author: mgblythe
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: mblythe
ms.custom: 
ms.openlocfilehash: 1e262fde37b68bcfcee3c974deb91bd07965de19
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="call-a-function-from-powerapps"></a>Wywoływanie funkcji z usługi PowerApps
[Rozwiązania PowerApps](https://powerapps.microsoft.com) platformy jest przeznaczona dla ekspertów biznesowych do tworzenia aplikacji bez kodu tradycyjnych aplikacji. Professional deweloperzy mogą używać usługi Azure Functions, aby rozszerzyć możliwości rozwiązania PowerApps, podczas osłaniania rozwiązania PowerApps konstruktorów aplikacji z szczegóły techniczne.

Należy utworzyć aplikację w tym temacie oparta na scenariuszu konserwacji dla turbin knie. W tym temacie przedstawiono sposób wywołania funkcji, który został zdefiniowany w [utworzyć definicję OpenAPI dla funkcji](functions-openapi-definition.md). Funkcja określa, czy awaryjnego naprawiania na turbiny knie jest ekonomiczne.

![Zakończono aplikację w rozwiązaniu PowerApps](media/functions-powerapps-scenario/finished-app.png)

Aby uzyskać informacje na wywoływanie tej samej funkcji z Flow firmy Microsoft, zobacz [wywoływanie funkcji z Microsoft Flow](functions-flow-scenario.md).

W tym temacie dowiesz się, jak:

> [!div class="checklist"]
> * Przygotuj przykładowych danych w programie Excel.
> * Eksportowanie definicji interfejsu API.
> * Dodaj połączenie do interfejsu API.
> * Utwórz aplikację, a następnie dodaj źródła danych.
> * Dodawanie formantów do wyświetlania danych w aplikacji.
> * Dodawanie formantów do wywołania funkcji i wyświetlania danych.
> * Uruchom aplikację, aby ustalić, czy naprawy ekonomicznego.

## <a name="prerequisites"></a>Wymagania wstępne

+ Aktywny [konta rozwiązania PowerApps](https://powerapps.microsoft.com/tutorials/signup-for-powerapps.md) przy użyciu tego samego konta w poświadczeniach jako konto platformy Azure. 
+ W programie Excel, ponieważ program Excel będzie używany jako źródło danych dla aplikacji.
+ Ukończ samouczek [utworzyć definicję OpenAPI dla funkcji](functions-openapi-definition.md).


## <a name="prepare-sample-data-in-excel"></a>Przygotowanie przykładowych danych w programie Excel
Możesz rozpocząć od przygotowania przykładowych danych, którego używasz w aplikacji. Poniższa tabela skopiować do programu Excel. 

| Tytuł      | Szerokość geograficzna  | Longtitude  | LastServiceDate | MaxOutput | ServiceRequired | EstimatedEffort | InspectionNotes                            |
|------------|-----------|-------------|-----------------|-----------|-----------------|-----------------|--------------------------------------------|
| Turbiny 1  | 47.438401 | -121.383767 | 2/23/2017       | 2850      | Tak             | 6               | Jest to drugi problem w tym miesiącu.       |
| Turbiny 4  | 47.433385 | -121.383767 | 5/8/2017        | 5400      | Tak             | 6               |                                            |
| Turbiny 33 | 47.428229 | -121.404641 | 6/20/2017       | 2800      |                 |                 |                                            |
| Turbiny 34 | 47.463637 | -121.358824 | 2/19/2017       | 2800      | Tak             | 7               |                                            |
| Turbiny 46 | 47.471993 | -121.298949 | 3/2/2017        | 1200      |                 |                 |                                            |
| Turbiny 47 | 47.484059 | -121.311171 | 8/2/2016        | 3350      |                 |                 |                                            |
| Turbiny 55 | 47.438403 | -121.383767 | 10/2/2016       | 2400      | Tak             | 40               | Mamy niektóre dostępne tej części. |

1. W programie Excel, wybierz dane, a na **Home** , kliknij pozycję **Format jako tabela**.

    ![W formacie tabeli](media/functions-powerapps-scenario/format-table.png)

1. Wybierz wszystkie style, a następnie kliknij przycisk **OK**.

1. Z tabelą wybrane na **projekt** wprowadź `Turbines` dla **nazwy tabeli**.

    ![Nazwa tabeli](media/functions-powerapps-scenario/table-name.png)

1. Skoroszyt programu Excel.

[!INCLUDE [Export an API definition](../../includes/functions-export-api-definition.md)]

## <a name="add-a-connection-to-the-api"></a>Dodaj połączenie do interfejsu API
Niestandardowy interfejs API (znanej także jako łącznik niestandardowy) jest dostępna w rozwiązaniu PowerApps, ale należy utworzyć połączenie z interfejsem API przed użyciem w aplikacji.

1. W [web.powerapps.com](https://web.powerapps.com), kliknij przycisk **połączenia**.

    ![Rozwiązanie PowerApps połączeń](media/functions-powerapps-scenario/powerapps-connections.png)

1. Kliknij przycisk **nowe połączenie**, przewiń w dół do **naprawy turbiny** łącznika i kliknij ją.

    ![Nowe połączenie](media/functions-powerapps-scenario/new-connection.png)

1. Wprowadź klucz interfejsu API, a następnie kliknij przycisk **Utwórz**.

    ![Utwórz połączenie](media/functions-powerapps-scenario/create-connection.png)

> [!NOTE]
> Możesz udostępniać aplikacji innym użytkownikom, każda osoba, która działa na lub korzysta z aplikacji należy także podać klucz interfejsu API, aby nawiązać połączenie z interfejsu API. To zachowanie może ulec zmianie w przyszłości, a aby odzwierciedlał, które będą aktualizowane.

## <a name="create-an-app-and-add-data-sources"></a>Utwórz aplikację, a następnie dodaj źródła danych
Teraz możesz przystąpić do tworzenia aplikacji w rozwiązaniu PowerApps i dodać dane programu Excel i niestandardowy interfejs API jako źródła danych dla aplikacji.

1. W [web.powerapps.com](https://web.powerapps.com), w okienku po lewej stronie kliknij **nowej aplikacji**.

1. W obszarze **pusta aplikacja**, kliknij przycisk **układ telefonu**.

    ![Tworzenie aplikacji typu tablet](media/functions-powerapps-scenario/create-phone-app.png)

    Aplikacja zostanie otwarty w rozwiązaniu PowerApps Studio dla sieci web. Na poniższej ilustracji przedstawiono różne części Studio rozwiązaniu PowerApps. Ten obraz jest gotowy aplikacji; pojawi się pusty ekran na początku w środkowym okienku.

    ![PowerApps Studio](media/functions-powerapps-scenario/powerapps-studio.png)

    **(1) na lewym pasku nawigacyjnym**, w którym można zobaczyć hierarchiczny widok wszystkich kontrolek na każdym ekranie

    **(2) środkowym okienku**, który wskazuje ekranu pracujesz nad

    **(3) po prawej**, których wartość opcje, takie jak układ i źródła danych

    **(4) właściwość** listy rozwijanej, w którym można wybrać właściwości, które dotyczą formuły

    **(5) pasek formuły**, gdzie dodać formuły (jak w programie Excel), które określają zachowanie aplikacji
    
    **[6] Wstążka**, którym Dodaj formanty i dostosować elementów projektu

1. Dodaj plik programu Excel jako źródła danych.

    1. W okienku po prawej stronie na **danych** , kliknij pozycję **Dodaj źródło danych**.

        ![Dodawanie źródła danych](media/functions-powerapps-scenario/add-data-source.png)

    1. Kliknij przycisk **Dodawanie statycznych danych do aplikacji**.

        ![Dodawanie źródła danych](media/functions-powerapps-scenario/add-static-data.png)

        Zwykle będzie odczytu i zapisu danych ze źródła zewnętrznego, ale dodajesz dane programu Excel jako dane statyczne, ponieważ to jest przykładowy.

    1. Przejdź do pliku programu Excel został zapisany, wybierz **turbin** tabeli, a następnie kliknij przycisk **Connect**.

        ![Dodawanie źródła danych](media/functions-powerapps-scenario/choose-table.png)

1. Dodaj niestandardowy interfejs API jako źródła danych.

    1. Na **danych** , kliknij pozycję **Dodaj źródło danych**.

    1. Kliknij przycisk **naprawy turbiny**.

        ![Turbiny naprawy łącznika](media/functions-powerapps-scenario/turbine-connector.png)

## <a name="add-controls-to-view-data-in-the-app"></a>Dodawanie formantów do wyświetlania danych w aplikacji
Teraz, źródła danych są dostępne w aplikacji, dodaniu do aplikacji, aby wyświetlić dane turbiny.

1. Na **Home** , kliknij pozycję **nowym ekranie** > **ekranu listy**.

    ![Ekran listy](media/functions-powerapps-scenario/list-screen.png)

    Rozwiązanie PowerApps dodaje ekranu, który zawiera *galerii* do wyświetlania elementów i inne formanty, które umożliwiają wyszukiwania, sortowania i filtrowania.

1. Pasek tytułu, aby zmienić `Turbine Repair`, a następnie zmień rozmiar galerii, więc ma miejsce dla kolejnych formantów w nim.

    ![Zmień tytuł, a następnie zmień rozmiar galerii](media/functions-powerapps-scenario/gallery-title.png)

1. Z Galerii wybranym w okienku po prawej stronie na **danych** Zmień źródła danych z **CustomGallerySample** do **turbin**.

    ![Źródło danych zmiany](media/functions-powerapps-scenario/change-data-source.png)

    Zestaw danych nie zawiera obrazu, dlatego dalej zmiany układu, aby lepiej dopasować dane. 

1. W okienku po prawej stronie Zmień **układu** do **tytuł, subtitle i treści**.

    ![Zmiana układu galerii](media/functions-powerapps-scenario/change-layout.png)

1. Jako ostatni krok w okienku po prawej stronie Zmień pola, które są wyświetlane w galerii.

    ![Zmień pola galerii](media/functions-powerapps-scenario/change-fields.png)
    
    + **Body1** = LastServiceDate
    + **Subtitle2** = ServiceRequired
    + **Tytuł2** = tytułu 

1. Wybrano galerii ustawienie **TemplateFill** właściwości do następującej formuły: `If(ThisItem.IsSelected, Orange, White)`.

    ![Wzór wypełnienia szablonu](media/functions-powerapps-scenario/formula-fill.png)

    Teraz jest widoczność elementu galerii jest zaznaczone.

    ![Wybrany element](media/functions-powerapps-scenario/selected-item.png)

1. Nie trzeba oryginalnego ekranu w aplikacji. W okienku po lewej stronie, umieść kursor nad **Screen1**, kliknij przycisk **...** , i **usunąć**.

    ![Usuń ekranu](media/functions-powerapps-scenario/delete-screen.png)

Wiele innych formatowania, które zwykle należy w aplikacji produkcyjnej, ale firma Microsoft będzie zająć się ważnym elementem w tym scenariuszu - wywołanie funkcji.

## <a name="add-controls-to-call-the-function-and-display-data"></a>Dodawanie formantów do wywołania funkcji i wyświetlania danych
Masz aplikację, która zawiera dane podsumowania dla każdego turbiny, więc teraz nadszedł czas na Dodaj formanty wywołania funkcji, który został utworzony, które przedstawiają dane są zwracane. Możesz uzyskać dostępu do funkcji, na podstawie sposobu jej nazwę w definicji OpenAPI; w takim przypadku ma `TurbineRepair.CalculateCosts()`.

1. Na wstążce na **Wstaw** , kliknij pozycję **przycisk**. Następnie na karcie tej samej kliknij **etykiety**

    ![Wstaw przycisków i etykiet](media/functions-powerapps-scenario/insert-controls.png)

1. Przeciągnij przycisk i etykiety projektowanie i zmienić rozmiar etykiety. 

1. Tekst przycisku Wybierz i zmień go na `Calculate costs`. Aplikacji powinien wyglądać podobnie jak na poniższej ilustracji.

    ![Aplikacji za pomocą przycisku](media/functions-powerapps-scenario/move-button-label.png)

1. Kliknij przycisk, a następnie wprowadź następującej formuły dla przycisku **OnSelect** właściwości.

    ```
    If (BrowseGallery1.Selected.ServiceRequired="Yes", ClearCollect(DetermineRepair, TurbineRepair.CalculateCosts({hours: BrowseGallery1.Selected.EstimatedEffort, capacity: BrowseGallery1.Selected.MaxOutput})))
    ```
    W tej formule wykonuje się, gdy przycisk zostanie kliknięty, a następnie wykonuje następujące czynności, jeśli ma zaznaczonego elementu galerii **ServiceRequired** wartość `Yes`:

    + Czyści *kolekcji* `DetermineRepair` do usunięcia danych z poprzedniego wywołania. Kolekcja jest zmienną tabelarycznych.

    + Przypisuje do zbierania danych zwróconych przez wywołanie funkcji `TurbineRepair.CalculateCosts()`. 
    
        Wartości przekazanych do funkcji pochodzą z **EstimatedEffort** i **MaxOutput** pól dla elementu wybranego w galerii. Te pola nie są wyświetlane w galerii, ale są one nadal dostępne do użycia w formułach.

1. Wybierz etykietę, a następnie wprowadź następującej formuły w etykiecie **tekst** właściwości.

    ```
    "Repair decision: " & First(DetermineRepair).message & " | Cost: " & First(DetermineRepair).costToFix & " | Revenue: " & First(DetermineRepair).revenueOpportunity
    ```
    Ta formuła `First()` funkcję, która ma dostęp do pierwszego (i tylko) wiersza `DetermineRepair` kolekcji. Następnie wyświetlane są trzy wartości, które zwraca funkcja: `message`, `costToFix`, i `revenueOpportunity`. Te wartości są puste, zanim aplikacja jest uruchamiana po raz pierwszy.

    Ukończonej aplikacji powinien wyglądać podobnie jak na poniższej ilustracji.

    ![Zakończono aplikację przed uruchomieniem](media/functions-powerapps-scenario/finished-app-before-run.png)


## <a name="run-the-app"></a>Uruchomienie aplikacji
Masz pełnej aplikacji. Teraz nadszedł czas, aby uruchomić go i nie wywołuje funkcji w akcji.

1. W prawym górnym rogu rozwiązania PowerApps Studio kliknij przycisk uruchamiania: ![Przycisk uruchamiania aplikacji](media/functions-powerapps-scenario/f5-arrow-sm.png).

1. Wybierz turbiny o wartości `Yes` dla **ServiceRequired**, następnie kliknij przycisk **obliczenie kosztów** przycisku. Powinny pojawić się wynik, podobnie jak na poniższej ilustracji.

    ![Zakończono aplikację w rozwiązaniu PowerApps](media/functions-powerapps-scenario/finished-app.png)

1. Spróbuj innych turbin, aby zobaczyć, co jest zwracane przez funkcję zawsze.

## <a name="next-steps"></a>Następne kroki
W tym temacie przedstawiono sposób:

> [!div class="checklist"]
> * Przygotuj przykładowych danych w programie Excel.
> * Eksportowanie definicji interfejsu API.
> * Dodaj połączenie do interfejsu API.
> * Utwórz aplikację, a następnie dodaj źródła danych.
> * Dodawanie formantów do wyświetlania danych w aplikacji.
> * Dodawanie formantów do wywołania funkcji i wyświetlania danych
> * Uruchom aplikację, aby ustalić, czy naprawy ekonomicznego.

Aby dowiedzieć się więcej na temat rozwiązania PowerApps, zobacz [wprowadzenie do rozwiązania PowerApps](https://powerapps.microsoft.com/tutorials/getting-started/).

Aby dowiedzieć się więcej o innych interesujące scenariuszy korzystających z usługi Azure Functions, zobacz [wywoływanie funkcji z Microsoft Flow](functions-flow-scenario.md) i [tworzy funkcję, która integruje się z usługą Azure Logic Apps](functions-twitter-email.md).