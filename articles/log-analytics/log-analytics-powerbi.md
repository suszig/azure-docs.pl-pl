---
title: "Importuj dane usługi Analiza dzienników Azure do usługi Power BI | Dokumentacja firmy Microsoft"
description: "Usługa Power BI jest oparte na chmurze usługi analizy biznesowej firmy Microsoft, który udostępnia zaawansowane wizualizacje i raporty do analizy różne zestawy danych.  W tym artykule opisano sposób konfigurowania importowania analizy dzienników danych do usługi Power BI i skonfigurować go do automatycznego odświeżania."
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
ms.date: 01/23/2018
ms.author: bwren
ms.openlocfilehash: e687a1ee8ac4f565062e57b07cdfa9ac5e6bbf4f
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
# <a name="import-azure-log-analytics-data-into-power-bi"></a>Importuj dane usługi Analiza dzienników Azure do usługi Power BI


[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) jest oparte na chmurze usługi analizy biznesowej firmy Microsoft, który udostępnia zaawansowane wizualizacje i raporty do analizy różne zestawy danych.  Wyniki wyszukiwania dziennika analizy dzienników można importować do zestawu danych usługi Power BI dzięki można korzystać z jego suchas funkcje w procesie łączenia danych z różnych źródeł i udostępnianie raportów w sieci web i urządzeń przenośnych.

Ten artykuł zawiera szczegółowe informacje na temat importowania danych analizy dzienników do usługi Power BI i planowania na automatyczne odświeżenie.  Różne procesy są uwzględnione w [uaktualnione](#upgraded-workspace) i [starszych](#legacy-workspace) obszaru roboczego.

## <a name="upgraded-workspace"></a>Uaktualniony obszaru roboczego


Aby zaimportować dane z [uaktualnione obszaru roboczego analizy dzienników](log-analytics-log-search-upgrade.md) do usługi Power BI można utworzyć zestawu danych w usłudze Power BI na podstawie zapytania wyszukiwania dziennika w analizy dzienników.  Zapytanie jest uruchamiane na każdym odświeżeniu zestawu danych.  Następnie można tworzyć raporty usługi Power BI, które używają danych z zestawu danych.  Można utworzyć zestawu danych w usłudze Power BI, możesz wyeksportować zapytania z analizy dzienników do [języka Power Query (M)](https://msdn.microsoft.com/library/mt807488.aspx).  Następnie wykorzystać te informacje, aby utworzyć zapytanie w programie Power BI Desktop, a następnie opublikować go do usługi Power BI jako zestawu danych.  Szczegóły tego procesu są opisane poniżej.

![Analiza dzienników do usługi Power BI](media/log-analytics-powerbi/overview.png)

### <a name="export-query"></a>Kwerendy eksportu
Rozpocznij od utworzenia [wyszukiwania dziennika](log-analytics-log-search-new.md) która zwraca dane z analizy dzienników chcesz wypełnić dataset usługi Power BI.  Następnie wyeksportować tego zapytania do [Power Query (M) języka](https://msdn.microsoft.com/library/mt807488.aspx) które mogą być używane przez program Power BI Desktop.

1. Utwórz wyszukiwania dziennika w analizy dzienników można wyodrębnić danych dla zestawu danych.
2. Jeśli używasz portalu dziennik wyszukiwania, kliknij przycisk **usługi Power BI**.  Jeśli używasz portalu usługi analiza wybierz **wyeksportować** > **Power BI kwerendy (M)**.  Zapytanie obu tych opcji eksportu w pliku tekstowym o nazwie **PowerBIQuery.txt**. 

    ![Eksportuj Dziennik wyszukiwania](media/log-analytics-powerbi/export-logsearch.png) ![Eksportuj Dziennik wyszukiwania](media/log-analytics-powerbi/export-analytics.png)

3. Otwórz plik tekstowy i skopiuj jej zawartość.

### <a name="import-query-into-power-bi-desktop"></a>Importowanie zapytania do Power BI Desktop
Power BI Desktop jest aplikacją, która umożliwia tworzenie zestawów danych i raportów, które mogą być publikowane do usługi Power BI.  Można również użyć do utworzenia zapytania przy użyciu języka dodatku Power Query wyeksportowane z analizy dzienników. 

1. Zainstaluj [Power BI Desktop](https://powerbi.microsoft.com/desktop/) , jeśli nie mają już go, a następnie otwórz aplikację.
2. Wybierz **Pobierz dane** > **puste zapytania** i otworzyć nowe zapytanie.  Następnie wybierz **Zaawansowany edytor** i Wklej zawartość wyeksportowany plik do zapytania. Kliknij przycisk **Gotowe**.

    ![Power BI Desktop zapytania](media/log-analytics-powerbi/desktop-new-query.png)

5. Uruchamia zapytanie, a jego wyniki są wyświetlane.  Może zostać wyświetlony monit o poświadczenia do połączenia z platformą Azure.  
6. Wpisz opisową nazwę zapytania.  Wartość domyślna to **Query1**. Kliknij przycisk **zamknąć i zastosować** można dodać zestaw danych do raportu.

    ![Power BI Desktop nazwy](media/log-analytics-powerbi/desktop-results.png)



### <a name="publish-to-power-bi"></a>Publikowanie do usługi Power BI
Podczas publikowania do usługi Power BI zostanie utworzony zestawu danych i raportu.  Jeśli utworzysz raport w programie Power BI Desktop, następnie ta jest publikowana z danymi.  Jeśli nie, następnie zostanie utworzony pusty raport.  Można zmodyfikować raport w usłudze Power BI lub Utwórz nowy, oparty na zestawie danych.

8. Tworzenie raportu na podstawie danych.  Użyj [dokumentacji Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-report-view) Jeśli nie znasz z nim.  Gdy wszystko jest gotowe do wysyłania do usługi Power BI, kliknij przycisk **publikowania**.  Po wyświetleniu monitu wybierz lokalizację docelową na koncie usługi Power BI.  Jeśli nie ma określonego miejsca docelowego, pamiętając, użyj **obszar Mój obszar roboczy**.

    ![Publikowanie Power BI Desktop](media/log-analytics-powerbi/desktop-publish.png)

3. Po zakończeniu publikowania, kliknij **Otwórz w usłudze Power BI** otworzyć usługi Power BI z nowego zestawu danych.


### <a name="configure-scheduled-refresh"></a>Skonfiguruj zaplanowane odświeżanie
Zestaw danych utworzone w usłudze Power BI będzie mieć tych samych danych, który wcześniej był wyświetlany w programie Power BI Desktop.  Należy odświeżyć zestawu danych, aby ponownie uruchomić zapytanie i wypełnić je przy użyciu najnowszych danych z analizy dzienników.  

1. Kliknij opcję w obszarze roboczym, w którym przekazać raport i wybierz **zestawów danych** menu. Wybierz menu kontekstowe obok nowy zestaw danych, a następnie wybierz **ustawienia**. W obszarze **poświadczenia źródła danych** powinien mieć komunikat, że poświadczenia są nieprawidłowe.  Jest to spowodowane nie zostały podane poświadczenia jeszcze dla zestawu danych do użycia podczas odświeżania danych.  Kliknij przycisk **Edycja poświadczeń** i określić poświadczenia z dostępem do analizy dzienników.

    ![Power BI harmonogramu](media/log-analytics-powerbi/powerbi-schedule.png)

5. W obszarze **zaplanowane odświeżanie** włączyć opcję **aktualności danych**.  Opcjonalnie można zmienić **częstotliwość odświeżania** i jeden lub więcej razy określone do uruchomienia odświeżania.

    ![Power BI odświeżania](media/log-analytics-powerbi/powerbi-schedule-refresh.png)

## <a name="legacy-workspace"></a>Starszej wersji obszaru roboczego
Podczas konfigurowania usługi Power BI z [starszej wersji obszaru roboczego analizy dzienników](log-analytics-powerbi.md), tworzenie kwerend dziennika wyeksportować wyniki do odpowiednich zestawów danych w usłudze Power BI.  Zapytania i eksportowanie nadal automatycznie uruchamiane zgodnie z harmonogramem, zdefiniowanego aktualności zestawu danych przy użyciu najnowszych danych zbieranych przez analizy dzienników.

![Analiza dzienników do usługi Power BI](media/log-analytics-powerbi/overview-legacy.png)

### <a name="power-bi-schedules"></a>Power BI harmonogramów
A *Power BI harmonogram* obejmuje wyszukiwania dziennika, który eksportuje zestaw danych z analizy dzienników do odpowiedniego zestawu danych w usłudze Power BI i harmonogram, który określa, jak często to wyszukiwanie jest uruchamiany aby zapewnić aktualność zestawu danych.

Pola w zestawie danych będzie odpowiadać właściwości rekordów zwróconych przez wyszukiwanie dziennika.  Jeśli wyniki wyszukiwania zawierają rekordy o różnych typach zestawu danych będzie zawierać wszystkie właściwości z poszczególnych typów rekordów uwzględnione.  

### <a name="connecting-log-analytics-workspace-to-power-bi"></a>Łączenie z usługą Power BI obszaru roboczego analizy dzienników
Przed analizy dzienników można wyeksportować do usługi Power BI, należy połączyć obszaru roboczego na koncie usługi Power BI, korzystając z następującej procedury.  

1. Kliknij w konsoli OMS **ustawienia** kafelka.
2. Wybierz **kont**.
3. W **informacje o obszarze roboczym** kliknij sekcję **nawiązywanie Power BI konta**.
4. Wprowadź poświadczenia dla konta usługi Power BI.

### <a name="create-a-power-bi-schedule"></a>Utwórz harmonogram Power BI
Utwórz harmonogram Power BI dla każdego zestawu danych, korzystając z następującej procedury.

1. Kliknij w konsoli OMS **wyszukiwania dziennika** kafelka.
2. Wpisz kwerendę lub wybierz zapisanego wyszukiwania, która zwraca dane, które mają zostać wyeksportowane do **usługi Power BI**.  
3. Kliknij przycisk **usługi Power BI** przycisk w górnej części strony, aby otworzyć **usługi Power BI** okna dialogowego.
4. Podaj informacje w poniższej tabeli i kliknij **zapisać**.

| Właściwość | Opis |
|:--- |:--- |
| Name (Nazwa) |Nazwę, aby zidentyfikować harmonogram, podczas wyświetlania listy harmonogramów usługi Power BI. |
| Zapisane wyszukiwanie |Wyszukiwanie dziennika do uruchomienia.  Można wybrać bieżącego zapytania lub wybierz istniejące zapisane wyszukiwanie w polu listy rozwijanej. |
| Harmonogram |Jak często uruchomić zapisane wyszukiwanie i eksportowanie z zestawem danych usługi Power BI.  Wartość musi należeć do zakresu od 15 minut do 24 godzin. |
| Nazwa zestawu danych |Nazwa zestawu danych w usłudze Power BI.  Zostanie ono utworzone, jeśli nie istnieje i zaktualizowany, jeśli istnieje. |

### <a name="viewing-and-removing-power-bi-schedules"></a>Wyświetlanie i usuwanie Power BI harmonogramów
Wyświetl listę istniejących Power BI harmonogramów z poniższej procedury.

1. Kliknij w konsoli OMS **ustawienia** kafelka.
2. Wybierz **Power BI**.

Oprócz szczegółów harmonogramu ile razy harmonogram zostało uruchomione w zeszłym tygodniu i stan ostatniej synchronizacji są wyświetlane.  Napotkano błędy synchronizacji po kliknięciu łącze, aby uruchomić wyszukiwanie rekordów dziennika przy użyciu szczegółów błędu.

Harmonogram można usunąć, klikając **X** w **kolumna usunąć**.  Harmonogram można wyłączyć, wybierając **poza**.  Aby zmodyfikować harmonogram należy ją usunąć i utworzyć go ponownie przy użyciu nowych ustawień.

![Power BI harmonogramów](media/log-analytics-powerbi/schedules.png)

### <a name="sample-walkthrough"></a>Przykładowe wskazówki
Poniższej sekcji przedstawiono przykład tworzenia harmonogramu Power BI i przy użyciu swojego zestawu danych, aby utworzyć prosty raport.  W tym przykładzie wszystkie dane dotyczące wydajności dla zestawu komputerów są eksportowane do usługi Power BI i wykres liniowy jest tworzony w celu wyświetlenia wykorzystaniu procesora.

#### <a name="create-log-search"></a>Tworzenie dziennika wyszukiwania
Firma Microsoft Rozpocznij od utworzenia dziennika Wyszukaj dane, które firma Microsoft ma zostać wysłany do zestawu danych.  W tym przykładzie użyjemy zapytanie zwracające wszystkie dane dotyczące wydajności na komputerach z nazwą rozpoczyna się od *srv*.  

![Power BI harmonogramów](media/log-analytics-powerbi/walkthrough-query.png)

#### <a name="create-power-bi-search"></a>Tworzenie Power BI wyszukiwania
Firma Microsoft kliknij **usługi Power BI** przycisk, aby otworzyć okno dialogowe usługi Power BI i podaj wymagane informacje.  Chcemy tego Wyszukaj, aby uruchomić jeden raz na godzinę i utworzyć zestawu danych o nazwie *wydajności Contoso*.  Ponieważ firma Microsoft już otwarte wyszukiwania utworzy dane chcemy, możemy Zachowaj ustawienie domyślne *Użyj bieżącego zapytania wyszukiwania* dla **zapisanych wyszukiwań**.

![Power BI wyszukiwania](media/log-analytics-powerbi/walkthrough-schedule.png)

#### <a name="verify-power-bi-search"></a>Sprawdź Power BI wyszukiwania
Aby sprawdzić, czy poprawnie utworzyliśmy harmonogram, możemy wyświetlić listę Power BI wyszukiwania w **ustawienia** kafelka na pulpicie nawigacyjnym OMS.  Firma Microsoft Poczekaj kilka minut i Odśwież tego widoku, dopóki nie zgłasza, że synchronizacja została uruchomiona.  Zwykle będzie zaplanować zestawu danych do automatycznego odświeżania.

![Power BI wyszukiwania](media/log-analytics-powerbi/walkthrough-schedules.png)

#### <a name="verify-the-dataset-in-power-bi"></a>Sprawdź zestaw danych w usłudze Power BI
Firma Microsoft Zaloguj się do naszej konta w [powerbi.microsoft.com](http://powerbi.microsoft.com/) i przewiń do **zestawów danych** w dolnej części okienka po lewej stronie.  Zobaczysz, że *wydajności Contoso* zestaw danych znajduje się wskazujący naszych eksportu zostało uruchomione pomyślnie.

![Power BI w zestawie danych](media/log-analytics-powerbi/walkthrough-datasets.png)

#### <a name="create-report-based-on-dataset"></a>Tworzenie raportu na podstawie w zestawie danych
Firma Microsoft wybierz **wydajności Contoso** zestawu danych, a następnie kliknij polecenie **wyniki** w **pola** w okienku po prawej stronie, aby wyświetlić dane, które są częścią tego zestawu danych.  Aby utworzyć wykres liniowy przedstawiający wykorzystanie procesora dla każdego komputera, możemy wykonać następujące czynności.

1. Wybierz wiersz wizualizacji wykresu.
2. Przeciągnij **ObjectName** do **filtru poziomu raportu** i sprawdź **procesora**.
3. Przeciągnij **CounterName** do **filtru poziomu raportu** i sprawdź **% czasu procesora**.
4. Przeciągnij **równowartości** do **wartości**.
5. Przeciągnij **komputera** do **legendy**.
6. Przeciągnij **TimeGenerated** do **osi**.

Zobaczysz, że wynikowa wykres liniowy jest wyświetlany przy użyciu danych z naszym zestawie danych.

![Wykres liniowy Power BI](media/log-analytics-powerbi/walkthrough-linegraph.png)

#### <a name="save-the-report"></a>Zapisz raport
Możemy zapisać raport, klikając przycisk Zapisz w górnej części ekranu i Zweryfikuj, że będzie teraz wyświetlany w sekcji raportów w okienku po lewej stronie.

![Power BI raportów](media/log-analytics-powerbi/walkthrough-report.png)



## <a name="next-steps"></a>Kolejne kroki
* Dowiedz się więcej o [dziennika wyszukiwania](log-analytics-log-searches.md) do tworzenia zapytań, które mogą być eksportowane do usługi Power BI.
* Dowiedz się więcej o [usługi Power BI](http://powerbi.microsoft.com) tworzenie wizualizacji opartych na eksportuje analizy dzienników.
