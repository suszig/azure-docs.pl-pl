---
title: "Power BI pulpit nawigacyjny kondycji vehicle i wspierającym zwyczaje - Azure | Dokumentacja firmy Microsoft"
description: "Korzystanie z możliwości Cortana Intelligence, aby uzyskać wgląd w czasie rzeczywistym oraz predykcyjnej na vehicle kondycji i wspierającym zwyczaje."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: aaeb29a5-4a13-4eab-bbf1-885690d86c56
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: bradsev
ms.openlocfilehash: 39be936520d62cb1c1c28de9bd72f8f489166082
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a>Instrukcje instalacji pulpitu nawigacyjnego programu Power BI szablon rozwiązania vehicle telemetrii analityka
To **menu** łącza do rozdziały tego podręcznika dotyczącego. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Rozwiązania analizy Telemetrii Vehicle ilustrację jak przedstawicielstw samochodu handlowych, producentów samochodów i ubezpieczeń mogą korzystać z funkcji Cortana Intelligence w celu uzyskania w czasie rzeczywistym i zwyczaje predykcyjnej rozeznanie vehicle kondycji i kierowania do ulepszenia dysku w obszarze klienta wystąpić R & D i kampanii marketingowych. Ten dokument zawiera instrukcje krok po kroku dotyczące sposobu konfigurowania raportów usługi Power BI i pulpitu nawigacyjnego po wdrożeniu rozwiązania w ramach subskrypcji. 

## <a name="prerequisites"></a>Wymagania wstępne
1. Wdrażanie [analizy Telemetrii](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90) rozwiązania  
2. [Zainstaluj program Microsoft Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331)
3. [Subskrypcji platformy Azure](https://azure.microsoft.com/pricing/free-trial/). Jeśli nie masz subskrypcji platformy Azure, Rozpoczynanie pracy z bezpłatnej subskrypcji Azure
4. Konto usługi Microsoft Power BI

## <a name="cortana-intelligence-suite-components"></a>Składników pakietu Cortana Intelligence
W ramach szablon rozwiązania analizy Telemetrii Vehicle następujące usługi Cortana Intelligence są wdrażane w ramach subskrypcji.

* **Centrum zdarzeń** służy do wprowadzania miliony zdarzeń telemetrii vehicle na platformie Azure.
* **Analiza strumienia** do uzyskania wgląd w czasie rzeczywistym w kondycję vehicle i będzie się powtarzał tych danych do długoterminowego przechowywania dla bardziej zaawansowane funkcje analizy partii.
* **Machine Learning** wykrywania anomalii w czasie rzeczywistym i przetwarzanie wsadowe, aby uzyskać wgląd predykcyjnej.
* **HDInsight** jest wykorzystywana do przekształcania danych na dużą skalę
* **Fabryka danych** obsługuje aranżacji, Planowanie zarządzania zasobami i monitorowania w potoku przetwarzania wsadowego.

**Power BI** daje to rozwiązanie sformatowanego pulpitu nawigacyjnego w czasie rzeczywistym danych i wizualizacji analizy predykcyjnej. 

Rozwiązanie używa dwóch różnych źródeł danych: **symulowane sygnały vehicle i danych diagnostycznych** i **katalogu vehicle**.

Symulator telematyki vehicle wchodzi w skład tego rozwiązania. Emituje informacje diagnostyczne, a sygnały odpowiadającej stan mechanizm i zwiększają wzorca w danym punkcie w czasie. 

Katalog Vehicle to odwołanie do zestawu danych zawierającego VIN do mapowania modelu

## <a name="power-bi-dashboard-preparation"></a>Power BI pulpitu nawigacyjnego przygotowania
### <a name="setup-power-bi-real-time-dashboard"></a>Pulpit nawigacyjny Instalatora Power BI w czasie rzeczywistym

**Uruchom aplikację w czasie rzeczywistym pulpitu nawigacyjnego** po zakończeniu wdrożenia należy postępuj zgodnie z instrukcjami działania ręczne

* Pobierz aplikację pulpitu nawigacyjnego w czasie rzeczywistym RealtimeDashboardApp.zip i Rozpakuj go.
*  W folderze rozpakowane Otwórz plik konfiguracji aplikacji 'RealtimeDashboardApp.exe.config' appSettings Zamień Eventhub, magazynu obiektów Blob i ML połączeń usługi z wartości w instrukcji działania ręczne i Zapisz zmiany.
* Uruchom aplikację RealtimeDashboardApp.exe. Podręczne, podaj prawidłowe poświadczenia usługi Power Bi i kliknij przycisk okna logowania **Akceptuj** przycisku. Następnie uruchomi aplikację do uruchamiania.

   ![Zaloguj się do usługi Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
   ![Power BI pulpitu nawigacyjnego uprawnień](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

* Zaloguj się do witryny sieci Web usługi Power Bi i utworzyć pulpit nawigacyjny w czasie rzeczywistym.

Teraz można przystąpić do konfigurowania pulpit nawigacyjny usługi Power BI z wizualizacjami sformatowanego uzyskanie w czasie rzeczywistym i zwyczaje predykcyjnej rozeznanie vehicle kondycji i wspierającym. Trwa około 45 minut na godziny, aby utworzyć wszystkich raportów i skonfigurować pulpitu nawigacyjnego. 

### <a name="configure-power-bi-reports"></a>Konfigurowanie raportów usługi Power BI
W czasie rzeczywistym raporty i pulpit nawigacyjny zająć około 30 do 45 minut, aby zakończyć. Przejdź do [http://powerbi.com](http://powerbi.com) i logowania.

![Zaloguj się do usługi Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

Nowy zestaw danych jest generowany w usłudze Power BI. Kliknij przycisk **ConnectedCarsRealtime** zestawu danych.

![Zestaw danych w czasie rzeczywistym samochodów połączone zaznaczeniu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

Zapisz przy użyciu pustego raportu **Ctrl + s**.

![Zapisz raport puste](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

Podaj nazwę raportu *Vehicle Telemetrii Analytics online w czasie rzeczywistym - raporty*.

![Podaj nazwę raportu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Raporty w czasie rzeczywistym
Istnieją trzy raporty w czasie rzeczywistym, w tym rozwiązaniu:

1. Pojazdów w operacji
2. Pojazdów wymagających konserwacji
3. Statystyki kondycji pojazdów

Można skonfigurować wszystkie trzy raporty w czasie rzeczywistym lub po każdym etapie i przejdź do następnej sekcji konfigurowania raportów partii. Firma Microsoft zaleca Utwórz trzy raporty w celu wizualizacji pełny wgląd w czasie rzeczywistym ścieżki rozwiązania.  

### <a name="1-vehicles-in-operation"></a>1. Pojazdów w operacji
Kliknij dwukrotnie **strona 1** i zmień jego nazwę na "Pojazdów w operacji"  
    ![Połączone samochodów - pojazdów w operacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

Wybierz **vin** pola **pola** i wybierz typ wizualizacji jako **"Karta"**.  

Wizualizacja karty jest tworzony, jak pokazano na rysunku.  
    ![Połączone samochodów — wybierz vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

Kliknij obszar puste, aby dodać nowej wizualizacji.  

Wybierz **miasta** i **vin** z pól. Zmień wizualizacji do **"Map"**. Przeciągnij **vin** w obszarze wartości. Przeciągnij **miasta** z pola do **legendy** obszaru.   
    ![Połączone samochodów — karta wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)

Wybierz **format** sekcji z **wizualizacje**, kliknij przycisk **tytuł** i zmienić **tekst** do **"pojazdów w operacji według miast"**.  
    ![Połączone samochodów - pojazdów w operacji przez miasta](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

Wizualizacja końcowego wygląda jak pokazano na rysunku.    
    ![Połączone samochodów - końcowego wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

Kliknij obszar puste, aby dodać nowej wizualizacji.  

Wybierz **miasta** i **vin**, Zmień typ wizualizacji do **wykres kolumnowy grupowany**. Upewnij się, **miasta** w **osi obszaru** i **vin** w **wartość obszaru**  

Wykres sortowania według **"Liczba vin"**  
    ![Połączone samochodów — liczba vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

Zmień wykres **tytuł** do **"Pojazdów w operacji według miast."**  

Kliknij przycisk **Format** sekcji, a następnie wybierz **kolory danych**, kliknij przycisk **"Na"** do **Pokaż wszystko**  
    ![Połączone samochodów — Pokaż wszystkie kolory danych](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

Zmiana koloru poszczególnych Miasto, klikając ikonę kolorów.  
    ![Połączone samochodów - Zmienianie kolorów](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

Kliknij obszar puste, aby dodać nowej wizualizacji.  

Wybierz **wykres kolumnowy grupowany** wizualizacji z wizualizacji, przeciągnij **miasta** w **osi** obszarze **modelu** w **Legendy** obszaru i **vin** w **wartość** obszaru.  
    ![Połączone samochodów - kolumnowy grupowany](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)  
    ![Połączone samochodów - renderowania](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)

Rozmieszczanie wszystkich wizualizacji na tej stronie, jak pokazano na rysunku.  
    ![Połączone samochodów - wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

Raport w czasie rzeczywistym "Pojazdów w operacji" została pomyślnie skonfigurowana. Możesz przejść do tworzenia następnej raportu w czasie rzeczywistym lub zatrzymać w tym miejscu i skonfigurować pulpitu nawigacyjnego. 

### <a name="2-vehicles-requiring-maintenance"></a>2. Pojazdów wymagających konserwacji
Kliknij przycisk ![Dodaj](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) Aby dodać nowy raport, zmienić jego nazwę na **"Pojazdów wymagających obsługi"**

![Połączone samochodów - pojazdów wymagających konserwacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

Wybierz **vin** pól i zmień typ wizualizacji do **karty**.  
    ![Połączone samochodów — karta Vin wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

Mamy pole o nazwie "MaintenanceLabel" w zestawie danych. To pole może mieć wartość "0" lub "1"." Ta wartość jest ustawiana przez model uczenia maszynowego Azure udostępniane w ramach rozwiązania i zintegrowane ze ścieżką w czasie rzeczywistym. Wartość "1" oznacza, że vehicle wymaga konserwacji. 

Aby dodać **poziomu strony** filtr przedstawiający pojazdów danych, które wymagają obsługi: 

1. Przeciągnij **"MaintenanceLabel"** pole do **filtrów stron poziomu**.  
   ![Połączone samochodów — filtry poziomu strony](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)  
2. Kliknij przycisk **podstawowe filtrowanie** w dolnej części filtru poziomu strony MaintenanceLabel menu.  
   ![Połączone samochodów — podstawowe filtrowanie](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)  
3. Ustaw dla niego wartość filtru **"1"**    
   ![Połączone samochodów — wartość filtru](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  

Kliknij obszar puste, aby dodać nowej wizualizacji.  

Wybierz **wykres kolumnowy grupowany** z wizualizacji  
![Połączone samochodów — karta Vind wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)  
![Połączone samochodów - kolumnowy grupowany](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

Przeciągnij pole **modelu** do **osi** obszarze **Vin** do **wartość** obszaru. Następnie Sortuj wizualizacji przez **liczba vin**.  Zmień wykres **tytuł** do **"Pojazdów wymagających obsługi przez model"**  

Przeciągnij **vin** pól do **nasycenie kolorów** w **pola** ![pola](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) sekcji **wizualizacji**kartę  
![Połączone samochodów - nasycenie kolorów](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

Zmień **kolory danych** w wizualizacji z **Format** sekcji  
Zmień kolor Minimum: **F2C812**  
Zmień kolor wartości maksymalnej do: **FF6300**  
![Połączone samochodów — zmiany kolorów](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)  
![Połączone samochodów — nowe kolory wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

Kliknij obszar puste, aby dodać nowej wizualizacji.  

Wybierz **klastrowanych kolumnowy** z wizualizacji, przeciągnij **vin** pole do **wartość** obszaru, przeciągnij **miasta** pole do **Osi** obszaru. Wykres sortowania przez **"Liczba vin"**. Zmień wykres **tytuł** do **"Pojazdów wymagających obsługi przez Miasto"**   
![Połączone samochodów - pojazdów wymagających obsługi przez miasta](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

Kliknij obszar puste, aby dodać nowej wizualizacji.  

Wybierz **karty wielowierszowych** wizualizacji z wizualizacji, przeciągnij **modelu** i **vin** do **pola** obszaru.  
![Połączone samochodów — karta wielowierszowych](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

Zmienianie rozmieszczenia wszystkie wizualizacji, raportu końcowego wygląda następująco:  
![Połączone samochodów — karta wielowierszowych](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

Raport w czasie rzeczywistym "Pojazdów wymagających obsługi" została pomyślnie skonfigurowana. Możesz przejść do tworzenia następnej raportu w czasie rzeczywistym lub zatrzymać w tym miejscu i skonfigurować pulpitu nawigacyjnego. 

### <a name="3-vehicles-health-statistics"></a>3. Statystyki kondycji pojazdów
Kliknij przycisk ![Dodaj](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png) Aby dodać nowy raport, zmienić jego nazwę na **"Statystyki kondycji pojazdów"**  

Wybierz **miernika** wizualizacji z wizualizacji, przeciągnij **szybkości** pole do **wartości, wartość minimalna, wartość maksymalna** obszarów.  
![Połączone samochodów — karta wielowierszowych](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

Zmień domyślny agregacji **szybkości** w **wartość obszaru** do **średni** 

Zmień domyślny agregacji **szybkości** w **obszar minimalny** do **minimalna**

Zmień domyślny agregacji **szybkości** w **maksymalny obszar** do **maksymalna**

![Połączone samochodów — karta wielowierszowych](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

Zmień nazwę **tytuł miernika** do **"Średnia szybkość"** 

![Połączone samochodów - miernika](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

Kliknij obszar puste, aby dodać nowej wizualizacji.  

Podobnie dodać **miernika** dla **średni wydobycie ropy naftowej aparat**, **średni paliwa**, i **aparat średnia temperatura**.  

Zmień agregacji domyślne pól w każdym miernika zgodnie powyżej kroki opisane w **"Średnia szybkość"** miernika.

![Połączone samochodów - mierników](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

Kliknij obszar puste, aby dodać nowej wizualizacji.

Wybierz **liniowy i kolumnowy grupowany** z wizualizacji, przeciągnij **miasta** pole do **udostępnionych osi**, przeciągnij **szybkości**, **pól tirepressure i engineoil** do **wartości w kolumnie** obszaru, zmień ich typ agregacji do **średni**. 

Przeciągnij **engineTemperature** pole do **wartości linii** obszaru, Zmień typ agregacji do **średni**. 

![Połączone samochodów - pola wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

Zmień wykres **tytuł** do **"Średnia szybkość, opona wykorzystania, aparat wydobycie ropy naftowej i temperatury aparat"**.  

![Połączone samochodów - pola wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

Kliknij obszar puste, aby dodać nowej wizualizacji.

Wybierz **właściwości** wizualizacji z wizualizacji, przeciągnij **modelu** pole do **grupy** obszaru, a następnie przeciągnij pole **MaintenanceProbability** do **wartości** obszaru.

Zmień wykres **tytuł** do **"Modele pojazdów wymagających obsługi"**.

![Połączone samochodów — Zmień tytuł wykresu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

Kliknij obszar puste, aby dodać nowej wizualizacji.

Wybierz **100% skumulowany wykres słupkowy** z wizualizacji, przeciągnij **miasta** pole do **osi** obszaru, a następnie przeciągnij **MaintenanceProbability**, **RecallProbability** pól do **wartość** obszaru.

![Połączone samochodów — Dodawanie nowej wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

Kliknij przycisk **Format**, wybierz pozycję **kolory danych**i ustaw **MaintenanceProbability** kolor wartości **"F2C80F"**.

Zmień **tytuł** wykresu, aby **"Prawdopodobieństwo z Vehicle konserwacji & Wycofaj przez Miasto"**.

![Połączone samochodów — Dodawanie nowej wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

Kliknij obszar puste, aby dodać nowej wizualizacji.

Wybierz **wykres warstwowy** z wizualizacji z wizualizacji, przeciągnij **modelu** pole do **osi** obszaru, a następnie przeciągnij **engineOil, tirepressure, szybkość i MaintenanceProbability** pól do **wartości** obszaru. Zmień ich typ agregacji do **"Średni"**. 

![Połączone samochodów — Zmień typ agregacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

Tytuł wykresu, aby zmienić **"Średni aparat wydobycie ropy naftowej, opona prawdopodobieństwo wykorzystania, szybkość i konserwacja przez model"**.

![Połączone samochodów — Zmień tytuł wykresu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

Kliknij obszar puste, aby dodać nowej wizualizacji:

1. Wybierz **wykres punktowy** wizualizacji z wizualizacji.
2. Przeciągnij **modelu** pole do **szczegóły** i **legendy** obszaru.
3. Przeciągnij **paliwa** pole do **osi x** obszaru, zmień agregacji do **średni**.
4. Przeciągnij **engineTemparature** do **osi y obszaru**, zmień agregacji do **średni**
5. Przeciągnij **vin** pole do **rozmiar** obszaru.

![Połączone samochodów — Dodawanie nowej wizualizacji](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

Zmień wykres **tytuł** do **"Średnie paliwa, aparat temperatury przez Model"**.

![Połączone samochodów — Zmień tytuł wykresu](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

Raport końcowy będzie wyglądać jak pokazano poniżej.

![Połączony raport końcowy samochodów](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>Wizualizacje numeru PIN z raportów do pulpitu nawigacyjnego w czasie rzeczywistym
Utwórz pusty pulpit nawigacyjny, klikając ikonę znaku plus obok pulpitów nawigacyjnych. Można określić nazwę "Pulpitu nawigacyjnego Analytics Vehicle Telemetrii"

![Połączone samochodów pulpitu nawigacyjnego](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

Kod PIN wizualizacji z powyższych raportów do pulpitu nawigacyjnego. 

![Połączone samochodów pulpitu nawigacyjnego](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

Pulpit nawigacyjny powinien wyglądać w następujący sposób od tego, są tworzone trzy raporty i odpowiednie wizualizacje są przypięty do pulpitu nawigacyjnego. Jeśli nie utworzono wszystkie raporty, pulpit nawigacyjny może wyglądać inaczej. 

![Połączone samochodów pulpitu nawigacyjnego](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Gratulacje! Pomyślnie utworzono w czasie rzeczywistym pulpitu nawigacyjnego. Jak wykonać CarEventGenerator.exe i RealtimeDashboardApp.exe, powinny pojawić się aktualizacje na żywo na pulpicie nawigacyjnym. Wykonaj poniższe kroki powinno zająć około 10 – 15 minut.

## <a name="setup-power-bi-batch-processing-dashboard"></a>Konfiguracja pulpitu nawigacyjnego przetwarzania wsadowego usługi Power BI
> [!NOTE]
> Trwa około dwie godziny (od pomyślnego ukończenia wdrożenia) dla potoku przetwarzania wsadowego kompleksowe na zakończenie wykonywania i przetworzyć roku warto wygenerowanych danych. Dlatego poczekaj na zakończenie przetwarzania przed wykonaniem dalszych kroków. 
> 
> 

**Pobierz plik projektanta usługi Power BI**

* Wstępnie skonfigurowane pliku projektanta usługi Power BI wchodzi w skład wdrożenia instrukcje działania ręczne
* Poszukaj 2. Instalator usługi Power BI partii przetwarzania w pulpicie nawigacyjnym można pobrać szablonu usługi Power BI dla pulpitu nawigacyjnego przetwarzania wsadowego nosi nazwę **ConnectedCarsPbiReport.pbix**.
* Zapisz lokalnie

**Konfigurowanie raportów usługi Power BI**

* Otwórz plik projektanta '**ConnectedCarsPbiReport.pbix**"przy użyciu Power BI Desktop. Jeśli nie masz już, zainstaluj Power BI Desktop z [instalacji Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331). 
* Kliknij przycisk **Edytuj zapytania**.

![Edytuj zapytanie usługi Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

* Kliknij dwukrotnie **źródła**.

![Źródło usługi Power BI zbioru](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

* Zaktualizuj parametry połączenia serwera z serwerem Azure SQL, które uzyskano udostępnione jako część wdrożenia.  Szukaj w zgodnie z instrukcjami dotyczącymi działania ręczne 

    4. Usługa Azure SQL Database
    
    * Serwer: somethingsrv.database.windows.net
    * Baza danych: connectedcar
    * Nazwa użytkownika: nazwa użytkownika
    * Hasło: Hasło programu SQL server można zarządzać z portalu Azure

* Pozostaw **bazy danych** jako *connectedcar*.

![Ustaw bazę danych usługi Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

* Kliknij przycisk **OK**.
* Zobaczysz **poświadczenia systemu Windows** kartę domyślnie zaznaczone, Zmień, aby **bazy danych poświadczeń** , klikając **bazy danych** kartę w prawym rogu.
* Podaj **Username** i **hasło** Twojej bazy danych SQL Azure, która została określona podczas jego wdrażania instalacji.

![Podaj poświadczenia bazy danych](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

* Kliknij przycisk **połączenia**
* Powtórz powyższe kroki dla każdego z trzech pozostałych kwerend w okienku po prawej stronie, a następnie zaktualizuj informacje dotyczące połączenia dla źródła danych.
* Kliknij przycisk **Zamknij i załadować**. Power BI Desktop pliku w zestawach danych są połączone z tabelami bazy danych SQL Azure.
* **Zamknij** pliku Power BI Desktop.

![Zamknij usługę Power BI desktop](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

* Kliknij przycisk **zapisać** przycisk, aby zapisać zmiany. 

Wszystkie raporty odpowiadający ścieżka przetwarzania wsadowego w rozwiązaniu został skonfigurowany. 

## <a name="upload-to-powerbicom"></a>Przekaż do *witrynie powerbi.com*
1. Przejdź do portalu sieci web usługi Power BI na http://powerbi.com i logowania.
2. Kliknij przycisk **pobieranie danych**  
3. Przekaż plik Power BI Desktop.  
4. Aby przekazać, kliknij przycisk **Pobierz dane -> pliki -> pliku lokalnego**  
5. Przejdź do **"**ConnectedCarsPbiReport.pbix**"**  
6. Gdy plik jest przekazywany, nastąpi przejście wstecz do obszaru roboczego usługi Power BI.  

Zestaw danych, raportów i pustego pulpitu nawigacyjnego zostanie utworzona automatycznie.  

Wykresy numeru PIN, aby nowy pulpit nawigacyjny o nazwie **pulpitu nawigacyjnego Analytics Telemetrii Vehicle** w **usługi Power BI**. Kliknij pozycję puste pulpit nawigacyjny utworzone powyżej, a następnie przejdź do **raporty** sekcji kliknij nowo przesłanym raport.  

![Vehicle Telemetrii Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

**Należy pamiętać, że raport ma sześć stron:**  
Strona 1: Gęstość Vehicle  
Strona 2: Kondycja vehicle w czasie rzeczywistym  
Strona 3: Agresywnie zmiennych pojazdów   
4 strony: Pojazdów odwołane  
5 strony: Efektywne zmiennych pojazdów paliwa  
6 strony: Logo firmy Contoso  

![Połączone samochodów Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

**Z 3 stron**, przypiąć następujące:  

1. Liczba VIN  
   ![Połączone samochodów Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 
2. Agresywnie zmiennych pojazdów przez model — wykresu wykresu kaskadowego  
   ![Vehicle Telemetrii - Pin wykresy 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

**Od 5 strony**, przypiąć następujące: 

1. Liczba vin    
   ![Vehicle Telemetrii - Pin wykresy 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2. Paliwa pojazdów wydajne przez model: kolumnowy grupowany  
   ![Vehicle Telemetrii - Pin wykresy 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

**Z 4 strony**, przypiąć następujące:  

1. Liczba vin  
   ![Vehicle Telemetrii - Pin wykresy 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 
2. Odwołane pojazdów miejscowość, model: właściwości  
   ![Vehicle Telemetrii - Pin wykresy 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

**Strona 6**, przypiąć następujące:  

1. Logo firmy Contoso silniki  
   ![Vehicle Telemetrii - Pin wykresy 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

**Organizowanie pulpitu nawigacyjnego**  

1. Przejdź do pulpitu nawigacyjnego
2. Umieść kursor nad każdy wykres i Zmień nazwę ją zależnie od nazw podane w poniższym obrazie Tworzenie pulpitu nawigacyjnego. Również poruszanie wykresy, aby wyglądały jak pulpit nawigacyjny poniżej.  
   ![Vehicle Telemetrii - organizowanie pulpitu nawigacyjnego 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
   ![Vehicle Telemetrii Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3. Jeśli utworzono wszystkie raporty, jak wspomniano w tym dokumencie, końcowego zakończonych pulpitu nawigacyjnego powinien wyglądać poniższej ilustracji. 

![Vehicle Telemetrii - organizowanie pulpitu nawigacyjnego 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Gratulacje! Pomyślnie utworzono raporty i pulpit nawigacyjny, aby uzyskać w czasie rzeczywistym, predykcyjnych i partii rozeznanie vehicle kondycji i wspierającym zwyczaje.  
