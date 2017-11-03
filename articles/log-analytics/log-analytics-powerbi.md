---
title: "Eksportuj dane analizy dzienników do usługi Power BI | Dokumentacja firmy Microsoft"
description: "Usługa Power BI jest oparte na chmurze usługi analizy biznesowej firmy Microsoft, który udostępnia zaawansowane wizualizacje i raporty do analizy różne zestawy danych.  Analiza dzienników stale wyeksportować dane z repozytorium OMS do usługi Power BI, można wykorzystać jej wizualizacje i narzędzia do analizy.  W tym artykule opisano sposób konfigurowania zapytań w analizy dzienników, która automatycznie eksportowania do usługi Power BI w regularnych odstępach czasu."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: bwren
ms.openlocfilehash: 271747e25f319c76195ec643025d24c6b7cdc9c5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="export-log-analytics-data-to-power-bi"></a>Eksportuj dane analizy dzienników do usługi Power BI

>[!NOTE]
> Jeśli obszaru roboczego został uaktualniony do [języka zapytań nowe analizy dzienników](log-analytics-log-search-upgrade.md), następnie tego procesu eksportowania analizy dzienników danych do usługi Power BI nie będzie dłużej działać.  Istniejących harmonogramów, które zostały utworzone przed rozpoczęciem uaktualniania zostanie wyłączony. Widoczne będzie także już możliwość Włącz możliwość eksportowania usługi Power BI w ustawieniach w wersji zapoznawczej, ta funkcja jest całkowicie zwolnione w uaktualnionym obszarów roboczych. 
>
> Po uaktualnieniu, analiza dzienników Azure używa tej samej platformy jako usługi Application Insights i używać tego samego procesu eksportowania analizy dzienników zapytań do usługi Power BI jako [procesu eksportowania zapytań usługi Application Insights do usługi Power BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).  Można albo eksportu zapytania przy użyciu konsoli analizy, zgodnie z opisem w tym artykule, lub wybrać **usługi Power BI** przycisk w górnej części ekranu w portalu wyszukiwania dziennika.
>
> Użytkownicy będą potrzebować dostępu do zasobów obszaru roboczego na platformie Azure do użycia usługi Power BI capabilitiy eksportu w uaktualnionym obszarów roboczych. Bez dostępu użytkownicy będą widzieć komunikat o błędzie podczas importowania zapytanie do usługi Power BI desktop, że nie masz dostępu.



[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) jest oparte na chmurze usługi analizy biznesowej firmy Microsoft, który udostępnia zaawansowane wizualizacje i raporty do analizy różne zestawy danych.  Analiza dzienników można automatycznie eksportować dane z repozytorium OMS do usługi Power BI, można wykorzystać jej wizualizacje i narzędzia do analizy.

Po skonfigurowaniu usługi Power BI z analizy dzienników, możesz utworzyć dziennika zapytań, które wyeksportować wyniki do odpowiednich zestawów danych w usłudze Power BI.  Zapytania i eksportowanie nadal automatycznie uruchamiane zgodnie z harmonogramem, zdefiniowanego aktualności zestawu danych przy użyciu najnowszych danych zbieranych przez analizy dzienników.

![Analiza dzienników do usługi Power BI](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a>Power BI harmonogramów
A *Power BI harmonogram* obejmuje wyszukiwania dziennika, które eksportuje zestaw danych z repozytorium OMS do odpowiedniego zestawu danych w usłudze Power BI i harmonogram, który określa, jak często to wyszukiwanie jest uruchamiany aby zapewnić aktualność zestawu danych.

Pola w zestawie danych będzie odpowiadać właściwości rekordów zwróconych przez wyszukiwanie dziennika.  Jeśli wyniki wyszukiwania zawierają rekordy o różnych typach zestawu danych będzie zawierać wszystkie właściwości z poszczególnych typów rekordów uwzględnione.  

> [!NOTE]
> Jest najlepszym rozwiązaniem jest użycie dziennika zapytania wyszukiwania, która zwraca dane pierwotne w przeciwieństwie do wykonywania konsolidacji przy użyciu poleceń, takich jak [miary](log-analytics-search-reference.md#measure).  Można wykonywać żadnych agregacji i obliczeń w usłudze Power BI z danych pierwotnych.
>
>

## <a name="connecting-oms-workspace-to-power-bi"></a>Obszar roboczy OMS nawiązywania połączenia z usługi Power BI
Przed analizy dzienników można wyeksportować do usługi Power BI, należy połączyć z obszarem roboczym pakietu OMS na koncie usługi Power BI, korzystając z następującej procedury.  

1. Kliknij w konsoli OMS **ustawienia** kafelka.
2. Wybierz **kont**.
3. W **informacje o obszarze roboczym** kliknij sekcję **nawiązywanie Power BI konta**.
4. Wprowadź poświadczenia dla konta usługi Power BI.

## <a name="create-a-power-bi-schedule"></a>Utwórz harmonogram Power BI
Utwórz harmonogram Power BI dla każdego zestawu danych, korzystając z następującej procedury.

1. Kliknij w konsoli OMS **wyszukiwania dziennika** kafelka.
2. Wpisz kwerendę lub wybierz zapisanego wyszukiwania, która zwraca dane, które mają zostać wyeksportowane do **usługi Power BI**.  
3. Kliknij przycisk **usługi Power BI** przycisk w górnej części strony, aby otworzyć **usługi Power BI** okna dialogowego.
4. Podaj informacje w poniższej tabeli i kliknij **zapisać**.

| Właściwość | Opis |
|:--- |:--- |
| Nazwa |Nazwę, aby zidentyfikować harmonogram, podczas wyświetlania listy harmonogramów usługi Power BI. |
| Zapisane wyszukiwanie |Wyszukiwanie dziennika do uruchomienia.  Można wybrać bieżącego zapytania lub wybierz istniejące zapisane wyszukiwanie w polu listy rozwijanej. |
| Harmonogram |Jak często uruchomić zapisane wyszukiwanie i eksportowanie z zestawem danych usługi Power BI.  Wartość musi należeć do zakresu od 15 minut do 24 godzin. |
| Nazwa zestawu danych |Nazwa zestawu danych w usłudze Power BI.  Zostanie ono utworzone, jeśli nie istnieje i zaktualizowany, jeśli istnieje. |

## <a name="viewing-and-removing-power-bi-schedules"></a>Wyświetlanie i usuwanie Power BI harmonogramów
Wyświetl listę istniejących Power BI harmonogramów z poniższej procedury.

1. Kliknij w konsoli OMS **ustawienia** kafelka.
2. Wybierz **Power BI**.

Oprócz szczegółów harmonogramu ile razy harmonogram zostało uruchomione w zeszłym tygodniu i stan ostatniej synchronizacji są wyświetlane.  Napotkano błędy synchronizacji po kliknięciu łącze, aby uruchomić wyszukiwanie rekordów dziennika przy użyciu szczegółów błędu.

Harmonogram można usunąć, klikając **X** w **kolumna usunąć**.  Harmonogram można wyłączyć, wybierając **poza**.  Aby zmodyfikować harmonogram należy ją usunąć i utworzyć go ponownie przy użyciu nowych ustawień.

![Power BI harmonogramów](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a>Przykładowe wskazówki
Poniższej sekcji przedstawiono przykład tworzenia harmonogramu Power BI i przy użyciu swojego zestawu danych, aby utworzyć prosty raport.  W tym przykładzie wszystkie dane dotyczące wydajności dla zestawu komputerów są eksportowane do usługi Power BI i wykres liniowy jest tworzony w celu wyświetlenia wykorzystaniu procesora.

### <a name="create-log-search"></a>Tworzenie dziennika wyszukiwania
Firma Microsoft Rozpocznij od utworzenia dziennika Wyszukaj dane, które firma Microsoft ma zostać wysłany do zestawu danych.  W tym przykładzie użyjemy zapytanie zwracające wszystkie dane dotyczące wydajności na komputerach z nazwą rozpoczyna się od *srv*.  

![Power BI harmonogramów](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a>Tworzenie Power BI wyszukiwania
Firma Microsoft kliknij **usługi Power BI** przycisk, aby otworzyć okno dialogowe usługi Power BI i podaj wymagane informacje.  Chcemy tego Wyszukaj, aby uruchomić jeden raz na godzinę i utworzyć zestawu danych o nazwie *wydajności Contoso*.  Ponieważ firma Microsoft już otwarte wyszukiwania utworzy dane chcemy, możemy Zachowaj ustawienie domyślne *Użyj bieżącego zapytania wyszukiwania* dla **zapisanych wyszukiwań**.

![Power BI wyszukiwania](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a>Sprawdź Power BI wyszukiwania
Aby sprawdzić, czy poprawnie utworzyliśmy harmonogram, możemy wyświetlić listę Power BI wyszukiwania w **ustawienia** kafelka na pulpicie nawigacyjnym OMS.  Firma Microsoft Poczekaj kilka minut i Odśwież tego widoku, dopóki nie zgłasza, że synchronizacja została uruchomiona.

![Power BI wyszukiwania](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-the-dataset-in-power-bi"></a>Sprawdź zestaw danych w usłudze Power BI
Firma Microsoft Zaloguj się do naszej konta w [powerbi.microsoft.com](http://powerbi.microsoft.com/) i przewiń do **zestawów danych** w dolnej części okienka po lewej stronie.  Zobaczysz, że *wydajności Contoso* zestaw danych znajduje się wskazujący naszych eksportu zostało uruchomione pomyślnie.

![Power BI w zestawie danych](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a>Tworzenie raportu na podstawie w zestawie danych
Firma Microsoft wybierz **wydajności Contoso** zestawu danych, a następnie kliknij polecenie **wyniki** w **pola** w okienku po prawej stronie, aby wyświetlić dane, które są częścią tego zestawu danych.  Aby utworzyć wykres liniowy przedstawiający wykorzystanie procesora dla każdego komputera, możemy wykonać następujące czynności.

1. Wybierz wiersz wizualizacji wykresu.
2. Przeciągnij **ObjectName** do **filtru poziomu raportu** i sprawdź **procesora**.
3. Przeciągnij **CounterName** do **filtru poziomu raportu** i sprawdź **% czasu procesora**.
4. Przeciągnij **równowartości** do **wartości**.
5. Przeciągnij **komputera** do **legendy**.
6. Przeciągnij **TimeGenerated** do **osi**.

Zobaczysz, że wynikowa wykres liniowy jest wyświetlany przy użyciu danych z naszym zestawie danych.

![Wykres liniowy Power BI](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-the-report"></a>Zapisz raport
Możemy zapisać raport, klikając przycisk Zapisz w górnej części ekranu i Zweryfikuj, że będzie teraz wyświetlany w sekcji raportów w okienku po lewej stronie.

![Power BI raportów](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [dziennika wyszukiwania](log-analytics-log-searches.md) do tworzenia zapytań, które mogą być eksportowane do usługi Power BI.
* Dowiedz się więcej o [usługi Power BI](http://powerbi.microsoft.com) tworzenie wizualizacji opartych na eksportuje analizy dzienników.
