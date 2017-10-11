---
title: "Jak monitorować usługi w chmurze | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak monitorować usługi w chmurze przy użyciu klasycznego portalu Azure."
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: 5c48d2fb-b8ea-420f-80df-7aebe2b66b1b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2015
ms.author: adegeo
ms.openlocfilehash: c369b22cf068a473343b006eb1b06fdd350d31db
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-monitor-cloud-services"></a>Jak monitorować usługi Cloud Services
[!INCLUDE [disclaimer](../../includes/disclaimer.md)]

Można monitorować `key` metryki wydajności dla usług w chmurze w klasycznym portalu Azure. Można ustawić poziom monitorowania minimalnego i pełne dla każdej usługi roli i dostosować Wyświetla monitorowania. Pełne dane monitorowania są przechowywane na koncie magazynu, które jest dostępne spoza portalu. 

Wyświetla monitorowania w klasycznym portalu Azure są konfigurowane w dużej. Można wybrać metryki mają być monitorowane na liście metryki na **Monitor** strony, a można wybrać, które metryk do wykreślenia w metryki wykresy na **Monitor** strony i pulpitu nawigacyjnego. 

## <a name="concepts"></a>Pojęcia
Domyślnie minimalnym monitorowania jest podawany nową usługę w chmurze za pomocą liczników wydajności zebranych z systemu operacyjnego hosta w wystąpieniach ról (maszyn wirtualnych). Minimalny metryki są ograniczone do procent użycia procesora CPU, dane w danych wychodzących, przepływność odczytu dysku i przepływność zapisu dysku. Konfigurując pełnego monitorowania może otrzymywać dodatkowe metryki na podstawie danych o wydajności na maszynach wirtualnych (wystąpienia roli). Pełne metryki Włącz analizę bliżej problemów występujących podczas działania aplikacji.

Domyślnie dane licznika wydajności z wystąpień roli jest próbkowany i przekazywane z wystąpienia roli co 3 minut. Po włączeniu pełnego monitorowania wydajności pierwotnych danych licznika jest agregowana częstotliwością wynoszącą 5 minut, godzinę i 12 godzin dla każdego wystąpienia roli, a w wystąpieniach ról dla każdej roli. Zagregowane dane są przeczyszczane po 10 dni.

Po włączeniu pełnego monitorowania zagregowane dane monitorowania są przechowywane w tabelach na koncie magazynu. Aby włączyć monitorowanie pełne dla roli, należy skonfigurować parametry połączenia diagnostyki prowadzący do konta magazynu. Można użyć różnych kont magazynu dla różnych ról.

Włączenie pełnego monitorowania zwiększa koszty magazynowania związane z magazynu danych, transfer danych i magazynu transakcji. Minimalny monitorowania nie wymaga konta magazynu. Nawet w przypadku ustawienia monitorowania poziom do pełnych danych dla metryki, które są dostępne na minimalnym poziomie monitorowania nie są przechowywane na koncie magazynu.

## <a name="how-to-configure-monitoring-for-cloud-services"></a>Porady: Konfigurowanie monitorowania dla usług w chmurze
Użyj poniższych procedur, aby skonfigurować, pełne lub minimalnym monitorowania w klasycznym portalu Azure. 

### <a name="before-you-begin"></a>Przed rozpoczęciem
* Utwórz *klasycznego* konta magazynu do przechowywania danych monitorowania. Można użyć różnych kont magazynu dla różnych ról. Aby uzyskać więcej informacji, zobacz [jak utworzyć konto magazynu](../storage/common/storage-create-storage-account.md#create-a-storage-account).
* Włącz diagnostyki Azure dla role usługi w chmurze. Zobacz [Konfigurowanie diagnostyki dla usługi w chmurze](cloud-services-dotnet-diagnostics.md).

Upewnij się, że parametry połączenia diagnostyki są dostępne w konfiguracji roli. Nie można włączyć pełne monitorowania do czasu włączenia diagnostyki Azure i obejmują parametry połączenia diagnostyki w konfiguracji roli.   

> [!NOTE]
> Projektach przeznaczonych dla 2.5 zestawu SDK platformy Azure nie obejmują automatycznie diagnostyki parametry połączenia w szablonie projektu. Te projekty musisz ręcznie dodać parametry połączenia diagnostyki do konfiguracji roli.
> 
> 

**Aby ręcznie dodać parametry połączenia diagnostyki do konfiguracji roli**

1. Otwórz projekt usługi w chmurze w programie Visual Studio
2. Kliknij dwukrotnie **roli** otworzyć projektanta i wybierz rolę **ustawienia** kartę
3. Wyszukaj ustawienie o nazwie **Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString**. 
4. Jeśli to ustawienie nie jest obecny, kliknij **Dodaj ustawienie** przycisk, aby dodać je do konfiguracji i zmienić typ nowe ustawienie do **ConnectionString**
5. Ustawianie wartości parametrów połączenia, klikając **...**  przycisku. Zostanie otwarte okno dialogowe umożliwiające wybranie konta magazynu.
   
    ![Ustawienia programu Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioDiagnosticsConnectionString.png)

### <a name="to-change-the-monitoring-level-to-verbose-or-minimal"></a>Aby zmienić poziom monitorowania pełne lub minimalnego
1. W [klasycznego portalu Azure](https://manage.windowsazure.com/), otwórz **Konfiguruj** strony dla wdrożenia usługi w chmurze.
2. W **poziom**, kliknij przycisk **pełne** lub **minimalnego**. 
3. Kliknij pozycję **Zapisz**.

Po włączeniu pełnego monitorowania powinny zacząć się wyświetlać dane monitorowania w klasycznym portalu Azure w ciągu godziny.

Dane licznika wydajności pierwotnych i zagregowanych danych monitorowania są przechowywane na koncie magazynu w tabelach kwalifikowana przez identyfikator wdrożenia dla ról. 

## <a name="how-to-receive-alerts-for-cloud-service-metrics"></a>Porady: otrzymywać alerty dotyczące metryk usługi w chmurze
Możesz otrzymywać alerty oparte na monitorowanie metryki usługi w chmurze. Na **usług zarządzania** strony klasyczny Portal Azure portalu, możesz utworzyć regułę do wyzwolenia alertu, gdy zostanie wybrana Metryka osiągnie wartość określonej. Można również mieć wiadomości e-mail wysyłane po wyzwoleniu alertu. Aby uzyskać więcej informacji, zobacz [porady: odbieranie powiadomień o alertach i Zarządzaj zasadami alertów w usłudze Azure](http://go.microsoft.com/fwlink/?LinkId=309356).

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Porady: Dodawanie metryk do tabeli metryk
1. W [klasycznego portalu Azure](http://manage.windowsazure.com/), otwórz **Monitor** strony dla usługi w chmurze.
   
    Domyślnie tabeli metryki przedstawia podzbiór dostępne metryki. Na ilustracji przedstawiono domyślne pełne metryki dla usługi w chmurze, które jest ograniczona do wartości licznika wydajności pamięć\dostępna pamięć (MB), dane zagregowane na poziomie roli. Użyj **dodać metryki** aby wybrać dodatkowe metryki agregacji i na poziomie roli, do monitorowania w klasycznym portalu Azure.
   
    ![Pełny ekran](./media/cloud-services-how-to-monitor/CloudServices_DefaultVerboseDisplay.png)
2. Aby dodać metryk do tabeli metryki:
   
   1. Kliknij przycisk **dodać metryki** otworzyć **wybrać metryki**, przedstawiono poniżej.
      
       Pierwszy dostępne metryki jest rozwinięty, aby wyświetlić opcje, które są dostępne. Dla każdego metryki opcję top Wyświetla zagregowane dane monitorowania dla wszystkich ról. Ponadto można wybrać poszczególnych ról, aby wyświetlić dane.
      
       ![Dodaj metryk](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics.png)
   2. Aby wybrać metryki, aby wyświetlić
      
      * Kliknij strzałkę w dół według metryki, aby rozwinąć opcje monitorowania.
      * Zaznacz pole wyboru dla każdej opcji monitorowania, które mają być wyświetlane.
        
        W tabeli metryki można wyświetlić metryk do 50.
        
        > [!TIP]
        > W ramach monitorowania pełne lista metryki może zawierać dziesiątki metryki. Aby wyświetlić paska przewijania, umieść kursor nad prawą stronę okna dialogowego. Aby filtrować listę, kliknij ikonę wyszukiwania, a w polu wyszukiwania wpisz tekst, jak pokazano poniżej.
        > 
        > 
        
        ![Dodaj wyszukiwanie metryk](./media/cloud-services-how-to-monitor/CloudServices_AddMetrics_Search.png)
3. Po wybraniu metryki, kliknij przycisk OK (znacznikiem wyboru).
   
    Wybrane metryki są dodawane do tabeli metryki, jak pokazano poniżej.
   
    ![Monitor metryk](./media/cloud-services-how-to-monitor/CloudServices_Monitor_UpdatedMetrics.png)
4. Aby usunąć metryki z tabeli metryki, kliknij przycisk metrykę, aby go zaznaczyć, a następnie kliknij przycisk **usunąć Metryka**. (Wyświetlane są tylko **usunąć Metryka** Jeśli masz wybraną metryką.)

### <a name="to-add-custom-metrics-to-the-metrics-table"></a>Aby dodać niestandardowe metryki do tabeli metryk
**Pełne** monitorowania poziomu zawiera listę domyślne metryki, które można monitorować w portalu. Oprócz tych można monitorować wszystkie metryki niestandardowe lub liczników wydajności zdefiniowanych przez aplikację za pośrednictwem portalu.

W następujących krokach założono, że włączono **pełne** poziom monitorowania i skonfigurowano do zbierania i transferu niestandardowe liczniki wydajności aplikacji. 

Aby wyświetlić liczniki wydajności niestandardowych w portalu, musisz zaktualizować konfiguracji w wad formantu kontenera:

1. Otwórz wad formantu kontenera obiektu blob na koncie magazynu diagnostyki. W tym celu można użyć programu Visual Studio lub innych Eksploratora magazynu.
   
    ![W Eksploratorze serwera programu Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioBlobExplorer.png)
2. Przejdź do ścieżki obiektu blob przy użyciu wzorca **DeploymentId/RoleName/RoleInstance** można znaleźć konfiguracji dla swojego wystąpienia roli. 
   
    ![Eksplorator usługi Storage programu Visual Studio](./media/cloud-services-how-to-monitor/CloudServices_Monitor_VisualStudioStorage.png)
3. Pobierz plik konfiguracji dla swojego wystąpienia roli i zaktualizować go, aby uwzględnić wszystkie niestandardowe liczniki wydajności. Na przykład do monitora *Bajty zapisu dysku/s* dla *dysku C* Dodaj następujący kod pod **PerformanceCounters\Subscriptions** węzła
   
    ```xml
    <PerformanceCounterConfiguration>
    <CounterSpecifier>\LogicalDisk(C:)\Disk Write Bytes/sec</CounterSpecifier>
    <SampleRateInSeconds>180</SampleRateInSeconds>
    </PerformanceCounterConfiguration>
    ```
4. Zapisz zmiany i przekazać plik konfiguracji do tej samej lokalizacji, zastępując istniejący plik w obiekcie blob.
5. Przełącz do trybu informacji pełnej w konfiguracji platformy Azure portalu klasycznego. Jeśli były w trybie informacji pełnej już konieczne będzie Przełącz do minimalnego i z powrotem do pełnego.
6. Licznik wydajności niestandardowe będą teraz dostępne w **dodać metryki** okno dialogowe. 

## <a name="how-to-customize-the-metrics-chart"></a>Porady: Dostosowywanie wykresu metryk
1. W tabeli metryki wybierz metryk do 6 do wykreślenia wykresu metryki. Aby wybrać metryki, kliknij pole wyboru jego lewej strony. Aby usunąć metryki z wykresu metryki, wyczyść jej pole wyboru w tabeli metryki.
   
    Po wybraniu metryki w tabeli metryki metryki są dodawane do wykresu metryki. Na ekranie wąskie **n więcej** listy rozwijanej zawiera nagłówki metryk, które nie pasują do wyświetlenia.
2. Aby przełączyć się między względne wartości (końcowa wartość tylko dla każdego metryki) i wartości bezwzględne (wyświetlane oś Y), wybierz względną lub bezwzględną w górnej części wykresu.
   
    ![Względna lub bezwzględna](./media/cloud-services-how-to-monitor/CloudServices_Monitor_RelativeAbsolute.png)
3. Aby zmienić zakres czasu wyświetla wykres metryki, wybierz godzinę, 24 godzin lub dni 7 w górnej części wykresu.
   
    ![Monitor okres wyświetlania](./media/cloud-services-how-to-monitor/CloudServices_Monitor_DisplayPeriod.png)
   
    Na wykresie metryki pulpitu nawigacyjnego metody do kreślenia metryki różni się. Standardowy zestaw metryki jest dostępna, i metryki zostały dodane lub usunięte przez wybranie metryki nagłówka.

### <a name="to-customize-the-metrics-chart-on-the-dashboard"></a>Aby dostosować wykres metryki na pulpicie nawigacyjnym
1. Otwórz pulpit nawigacyjny dla usługi w chmurze.
2. Dodaj lub usuń metryki z wykresu:
   
   * Do wykreślenia nowe metryki, zaznacz pole wyboru metryki w nagłówkach wykresu. Na ekranie wąskie, kliknij strzałkę w dół przez  ***n* ? metryki** do wykreślenia metrykę obszaru wykresu, do nagłówka nie może wyświetlić.
   * Aby usunąć metrykę, które są kreślone na wykresie, wyczyść pole wyboru przy jej nagłówek.
   
3. Przełączanie między **względną** i **bezwzględne** Wyświetla.
4. Wybierz godzinę, 24 godzin lub 7 dni danych do wyświetlenia.

## <a name="how-to-access-verbose-monitoring-data-outside-the-azure-classic-portal"></a>Porady: dostęp do pełnego monitorowania danych poza klasycznego portalu Azure
Pełne dane monitorowania są przechowywane w tabelach na kontach magazynu, które określają dla każdej roli. Dla każdego wdrożenia usługi chmury sześciu tabele są tworzone dla roli. Dwie tabele są tworzone dla każdej (5 minut, godzinę i 12 godzin). Jeden z tych tabel przechowuje agregacji na poziomie roli; drugiej tabeli przechowuje agregacji dla wystąpień roli. 

Nazwy tabeli mają następujący format:

```
WAD*deploymentID*PT*aggregation_interval*[R|RI]Table
```

Gdzie:

* *deploymentID* jest identyfikatorem GUID przypisany do wdrożenia usługi w chmurze
* *aggregation_interval* = 5 M, 1 H lub 12 H
* agregacji na poziomie roli = R
* agregacji dla wystąpień roli = RI

Na przykład poniższe tabele będzie przechowywać pełne dane monitorowania zagregowane na 1 godzinę:

```
WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRTable (hourly aggregations for the role)

WAD8b7c4233802442b494d0cc9eb9d8dd9fPT1HRITable (hourly aggregations for role instances)
```
