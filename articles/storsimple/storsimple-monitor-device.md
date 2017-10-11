---
title: "Monitorowanie urządzenia StorSimple | Dokumentacja firmy Microsoft"
description: "Informacje dotyczące używania usługi Menedżer StorSimple do monitorowania wydajności operacji We/Wy, wykorzystanie pojemności, przepływność sieci i wydajność urządzenia."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: bd4f7704-4f6f-47d0-927a-b1c91eabc453
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/16/2016
ms.author: alkohli
ms.openlocfilehash: d45bb37c8417785db0ea38be4375a998b6d9f109
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-storsimple-manager-service-to-monitor-your-storsimple-device"></a>Użyj usługi Menedżer StorSimple do monitorowania urządzenia StorSimple
## <a name="overview"></a>Omówienie
Usługę Menedżer StorSimple służy do monitorowania określonych urządzeń w rozwiązaniu StorSimple. Można tworzyć niestandardowe wykresy na podstawie wydajności operacji We/Wy, wykorzystanie pojemności, przepływność sieci i metryki wydajności urządzenia. 

Aby wyświetlić dane monitorowania dla określonego urządzenia, w klasycznym portalu Azure, wybierz usługę Menedżer StorSimple. Kliknij przycisk **Monitor** , a następnie wybierz z listy urządzeń. **Monitor** strona zawiera następujące informacje.

## <a name="io-performance"></a>Wydajność We/Wy
**Wydajność We/Wy** śledzi metryk powiązanych z liczbą odczytu i zapisu między albo interfejsy inicjatora iSCSI na serwerze hosta i urządzenie lub urządzenia i chmury. To wydajność może być zmierzony dla określonego woluminu, określony wolumin kontenera lub wszystkie kontenery woluminów.

Poniższej tabeli przedstawiono we/wy dla inicjatora na urządzeniu dla wszystkich woluminów urządzenia produkcji. Metryki wykreślić są do odczytu i zapisu bajtów na sekundę, odczytu i operacji We/Wy na sekundę, zapisu i odczytu i zapisu opóźnienia.

![Wydajność We/Wy z inicjatora dla urządzenia](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_InitiatorTODevice_For_AllVolumesM.png)

Dla tego samego urządzenia operacje We/Wy są kreślone danych z urządzenia do chmury dla wszystkich kontenerów woluminu. Na tym urządzeniu dane są tylko w warstwie liniowej i nic nie ma rozrzucone do chmury. Nie ma żadnych operacje odczytu i zapisu występujących z urządzenia do chmury. W związku z tym pików na wykresie są co 5 minut umożliwiająca częstotliwość sprawdzania pulsu między urządzeniem i usługi. 

![Wydajność We/Wy z urządzenia do chmury](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainersM.png)

Dla tego samego urządzenia chmury migawki danych woluminu, zaczynając od 2:00 pm. To sprawia, że dane przepływające z urządzenia do chmury. Odczytuje i zapisuje zostały obsłużone do chmury w ten czas trwania. Wykres we/wy pokazuje szczytu w różnych metryk odpowiadający czas, kiedy migawki. 

![Wydajność We/Wy urządzenia do chmury po migawka w chmurze](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainers2M.png)

## <a name="capacity-utilization"></a>Użycie wydajności
**Użycie wydajności** śledzi metryki dotyczące ilości miejsca do magazynowania danych, używany przez woluminy, kontenery woluminów lub urządzenia. Możesz utworzyć raporty na podstawie wykorzystania pojemności magazynu podstawowego, magazynu w chmurze lub pamięć masową urządzenia. Wykorzystanie pojemności może być zmierzony przy określonego woluminu, określony wolumin kontenera lub wszystkie kontenery woluminów.

Chmury podstawowej, a pojemność magazynu urządzenia można przedstawić w następujący sposób:

### <a name="primary-storage-capacity-utilization"></a>Podstawowe wykorzystanie pojemności
Te na wykresach ilość danych zapisywane woluminów StorSimple przed danych jest deduplikowany i skompresowane. Wykorzystanie magazynu głównej można wyświetlić wszystkich woluminów lub jednego woluminu.

Podczas przeglądania wykresy wykorzystania pojemności magazynu głównego woluminu wszystkie woluminy w porównaniu z każdej z poszczególnych woluminów i sumowanie danych podstawowych w obu przypadkach, może być niezgodność między dwiema liczbami. Całkowita danych podstawowych we wszystkich woluminach nie można dodać do całkowitej ilości danych podstawowych poszczególnych woluminów. Może to być spowodowane jedną z następujących czynności:

* **Dane uwzględnione dla wszystkich woluminów migawek**: ten problem występuje tylko wtedy, gdy używasz wersji wcześniejszych niż aktualizacja Update 3. Podstawowe dane wyświetlane dla wszystkich woluminów, które jest sumą danych podstawowych dla każdego woluminu i dane migawki. Podstawowe dane wyświetlane dla danego woluminu odpowiada tylko ilość danych przydzielone w woluminie (i nie ma odpowiednich danych migawek woluminu).
  
    Można to również wyjaśnić przez następujące równanie:
  
    *Danych podstawowych (wszystkich woluminów) = Suma (danych podstawowych (woluminu i) + rozmiar danych migawki (woluminu i))*
  
    *w przypadku gdy podstawowy danych (woluminu i) = rozmiar danych podstawowych przydzielone do woluminu i*
  
    Usunięcie migawki za pomocą usługi usunięcie odbywa się asynchronicznie w tle. Może upłynąć trochę czasu, rozmiar danych woluminu, należy uaktualnić po usunięciu migawki. 
  
    Jeśli uruchomiona aktualizacji 3 lub nowszym, dane migawki nie jest uwzględniony w danych woluminu. I podstawowego wykorzystania jest obliczana w następujący sposób:
  
    * Głównej danych (wszystkich woluminów) = Suma (danych podstawowych (woluminu i)
  
    *w przypadku gdy podstawowy danych (woluminu i) = rozmiar danych podstawowych przydzielone do woluminu i*
* **Woluminy z monitorowaniem wyłączone objęte wszystkie woluminy**: Jeśli masz woluminów na urządzeniu, dla których monitorowanie jest wyłączone, dane monitorowania tych poszczególnych woluminów nie jest dostępny w wykresy. Jednak dane wszystkie woluminy na wykresie obejmują woluminów, których monitorowanie jest wyłączone. 
* **Usunięte z uwzględnione wszystkie woluminy na żywo skojarzonych kopii zapasowych woluminów**: Jeśli woluminy zawierające dane migawki są usuwane, ale nadal istnieją skojarzone migawki, a następnie niezgodność może zostać wyświetlony.
* **Usunięte woluminy uwzględnione dla wszystkich woluminów**: W niektórych przypadkach mogą istnieć starszych woluminów, nawet jeśli te zostały usunięte. Wpływ usunięcia nie jest widoczny i urządzenia mogą być wyświetlane niższe dostępnej pojemności. Należy skontaktować się z Microsoft Support do usunięcia tych woluminów.

Następujące na wykresach wykorzystanie pojemności magazynu głównego urządzenia StorSimple przed i po migawki chmury. Jak jest to po prostu danych woluminu, migawka w chmurze nie należy zmieniać magazynu głównego. Jak widać, wykres ten przedstawia różnicy w wykorzystania pojemności głównej wyniku migawki chmury. Migawka w chmurze rozpoczęty o godzinie około 2:00 pm na tym urządzeniu.

![Wykorzystanie pojemności głównej przed migawka w chmurze](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes2M.png)

![Wykorzystanie pojemności głównej po migawka w chmurze](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes1M.png)

Jeśli używasz Update 2 lub nowszym, można przerwać dół wykorzystania pojemności magazynu głównego poszczególnych woluminów, wszystkie woluminy, wszystkie woluminy warstwowe i wszystkich woluminów przypiętych lokalnie jak pokazano poniżej. Dzielenie przez wszystkich woluminów przypiętych lokalnie pozwoli szybko ustalić, ile warstwie lokalnej jest wykorzystane.

![Wykorzystanie pojemności głównej dla wszystkich woluminów przypiętych lokalnie](./media/storsimple-monitor-device/localvolumes.png)

### <a name="cloud-storage-capacity-utilization"></a>Wykorzystanie pojemność magazynu w chmurze
Te na wykresach wielkość magazynu w chmurze używane. Te dane są deduplikowane i skompresowane. Ta wartość obejmuje migawki w chmurze, które mogą zawierać dane, które nie jest uwzględniana w głównej woluminami i jest przechowywana na potrzeby przechowywania starszych lub wymagane. Możesz porównać podstawową i chmury magazynu danych, aby poznać zmniejszenie szybkości danych, mimo że nie liczba będzie równa dokładnie. Wykorzystanie pojemność magazynu chmury urządzenia StorSimple przed i po chmurze migawki wykresach. Migawka w chmurze rozpoczęty o godzinie około 2:00 pm na tym urządzeniu, aby zobaczyć wykorzystanie pojemności chmury zrzut w tym samym czasie, zwiększenie z 5.73 MB, GB 4.04.

![Wykorzystanie pojemności chmury przed migawka w chmurze](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers2M.png)

![Wykorzystanie pojemności chmury po migawka w chmurze](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers1M.png)

### <a name="device-storage-capacity-utilization"></a>Wykorzystanie pojemności urządzenia
Te na wykresach łączne użycie urządzenia, którego będzie użycie więcej niż podstawowy, ponieważ zawiera on liniowej warstwy dysków SSD. Ta warstwa zawiera ilość danych istnieje również na urządzenia do innej warstwy. Ponownym włączeniu pojemności w liniowej warstwy dysków SSD, dzięki czemu mają nowe dane, stare dane jest przenoszony do warstwy dysków HDD (po tym czasie jest deduplikowany i skompresowane), a następnie do chmury.

Wraz z upływem czasu użycia głównej pojemności i wykorzystania pojemności urządzenia najprawdopodobniej zwiększy razem do momentu rozpoczęcia dane do należeć do warstwy do chmury. W tym momencie prawdopodobnie rozpocznie wykorzystanie pojemności urządzenia Poziomowanie, ale wykorzystanie pojemności podstawowego spowoduje zwiększenie jako zapisywanych jest więcej danych.

Następujące na wykresach wykorzystanie pojemności magazynu głównego urządzenia StorSimple przed i po migawki chmury. Migawka w chmurze zaczęła godzinie 2:00 a wykorzystanie pojemności urządzenia zmniejszenie o tej godzinie. Wykorzystanie pojemność magazynu urządzenia zakończył działanie z 11.58 GB 7.48 GB. To wskazuje, że prawdopodobnie nieskompresowanych danych w warstwie dysków SSD liniowej został deduplikowane, skompresowane i przeniesione do warstwy dysków HDD. Należy pamiętać, że jeśli urządzenie już dużej ilości danych w warstwach zarówno dysków SSD i HDD, mogą nie być widoczne to zmniejszenie. W tym przykładzie urządzenie ma niewielką ilość danych.

![Wykorzystanie pojemności urządzenia przed migawka w chmurze](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil2M.png)

![Wykorzystanie pojemności urządzenia po migawka w chmurze](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil1M.png)

## <a name="network-throughput"></a>Przepustowość sieci
**Przepustowość sieci** śledzi metryki dotyczące ilość danych przesyłanych z interfejsów sieciowych inicjatora iSCSI na serwerze hosta, a urządzenie i między urządzeniem i chmurą. Ta metryka dla poszczególnych interfejsów sieciowych iSCSI można monitorować na urządzeniu.

Poniższych wykresach przepustowość sieci interfejsu dane 0 a 4 danych, obu 1 GbE interfejsów na urządzeniu. W tym przypadku Data 0 został włączoną obsługę chmury 4 danych został włączony iSCSI. Widać przychodzącego i wychodzącego ruchu sieciowego dla urządzenia StorSimple. Płaski wiersza na wykresie, począwszy od 15:24:00 jest ze względu na fakt, firma Microsoft zbiera dane o co 5 minut i należy ją ignorować. 

![Przepustowość sieci dla Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data0M.png)

![Przepustowość sieci dla Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data4M.png)

## <a name="device-performance"></a>Wydajność urządzenia
**Wydajność urządzenia** śledzi metryki związane z wydajnością urządzenia. W poniższej tabeli przedstawiono Statystyka wykorzystania procesora CPU dla urządzenia w środowisku produkcyjnym.

![Użycie procesora CPU dla urządzenia](./media/storsimple-monitor-device/StorSimple_DeviceMonitor_DevicePerformance1M.png)

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [pulpit nawigacyjny urządzenia usługi Menedżer StorSimple](storsimple-device-dashboard.md).
* Dowiedz się, jak [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md).

