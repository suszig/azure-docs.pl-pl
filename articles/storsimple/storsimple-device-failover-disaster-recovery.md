---
title: StorSimple trybu failover i odzyskiwania po awarii | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak urządzenia StorSimple, inne urządzenie fizyczne lub urządzenie wirtualne w tryb failover."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5751598e-49c8-42b3-8121-fea5857a7d83
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/16/2016
ms.author: alkohli
ms.openlocfilehash: bf92ffdb16b86c4033cc96ae2abb060d90f9505e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="failover-and-disaster-recovery-for-your-storsimple-device"></a>Tryb failover i odzyskiwania po awarii dla urządzenia StorSimple
## <a name="overview"></a>Omówienie
W tym samouczku opisano kroki wymagane do pracy awaryjnej urządzenia StorSimple na wypadek awarii. Tryb failover umożliwia migrację danych z urządzenia źródłowego w centrum danych do innego fizycznego lub nawet urządzenie wirtualne znajdujące się w tej samej lub innej lokalizacji geograficznej. 

Odzyskiwania awaryjnego (DR) jest zorkiestrowana za pomocą funkcji trybu failover urządzenia i zainicjowano **urządzeń** strony. Ta strona rejestruje wszystkie urządzenia StorSimple połączona z usługą Menedżer StorSimple. Dla każdego urządzenia są wyświetlane przyjazna nazwa, stan, elastycznie i maksymalną pojemność, typ i modelu.

![Strona urządzenia](./media/storsimple-device-failover-disaster-recovery/IC740972.png)

Wskazówki zawarte w tym samouczku dotyczą urządzeń fizycznych i wirtualnych StorSimple przez wszystkie wersje oprogramowania.

## <a name="disaster-recovery-dr-and-device-failover"></a>Odzyskiwania awaryjnego (DR) i urządzenia trybu failover
W przypadku odzyskiwanie po awarii urządzenie podstawowe przestanie działać. W takiej sytuacji można przenieść dane chmury skojarzone z urządzeniem nie powiodło się na innym urządzeniu przy użyciu podstawowego urządzenia jako *źródła* i określając inne urządzenie jako *docelowej*. Można wybrać co najmniej jeden kontenery woluminów, aby przeprowadzić migrację na urządzeniu docelowym. Ten proces jest nazywany *pracy awaryjnej*. 

Podczas pracy awaryjnej kontenery woluminów z urządzenia źródłowego zmieniają właściciela i są przenoszone do urządzenia docelowego. Po kontenery woluminów zmienić własność, są one usuwane z urządzenia źródłowego. Po zakończeniu usuwania urządzenia docelowego można następnie można przełączyć ponownie.

Zwykle po DR najnowszej kopii zapasowej służy do przywracania danych na urządzeniu docelowym. Jednak w przypadku wielu zasad tworzenia kopii zapasowych na tym samym woluminie, następnie pobiera pobrania zasad tworzenia kopii zapasowej z największą liczbą woluminów i najnowszej kopii zapasowej z ta zasada służy do przywracania danych na urządzeniu docelowym.

Na przykład jeśli istnieją dwie zasady tworzenia kopii zapasowej (jeden domyślny i jeden niestandardowy) *defaultPol*, *customPol* z następującymi szczegółami:

* *defaultPol* : jeden wolumin *vol1*, jest uruchamiany codziennie, zaczynając od 22:30:00.
* *customPol* : cztery woluminy *vol1*, *vol2*, *vol3*, *vol4*, jest uruchamiany codziennie, zaczynając od 10:00 PM.

W takim przypadku *customPol* będzie służyć jako ma więcej woluminów i z różnym priorytetem, spójności awarii. Najnowszej kopii zapasowej z tych zasad służy do przywracania danych.

## <a name="considerations-for-device-failover"></a>Zagadnienia dotyczące urządzenia trybu failover
W przypadku awarii możesz przełączyć urządzenia StorSimple:

* Na urządzeniu fizycznym 
* Do samego siebie
* Na urządzenie wirtualne

Dla dowolnego urządzenia trybu failover należy pamiętać następujące czynności:

* Wymagania wstępne dotyczące odzyskiwania po awarii są, że wszystkie woluminy w kontenery woluminów są w trybie offline i kontenery woluminów ma skojarzoną migawkę w chmurze. 
* Urządzenia dostępnych do odzyskiwania po awarii są urządzenia, które mają wystarczającą ilość miejsca, aby pomieścić kontenery wybranego woluminu. 
* Urządzenia, które są połączone z usługą, ale nie spełniają kryteria wystarczającą ilość miejsca nie będą dostępne jako urządzenia docelowe.
* Następujące odzyskiwania po awarii na czas, wydajność dostępu do danych może mieć wpływ znacznie, jak urządzenie będzie musiał uzyskać dostęp do danych z chmury, a także przechowywane lokalnie.

#### <a name="device-failover-across-software-versions"></a>Tryb failover urządzeń między wersjami oprogramowania
W ramach wdrożenia usługi Menedżer StorSimple może mieć wielu urządzeń fizycznych i wirtualnych, wszystkie wersje uruchamianie innego oprogramowania. W zależności od wersji oprogramowania typy woluminu na urządzeniach, może być również inny. Na przykład urządzenie z uruchomioną Update 2 lub nowszego lokalnie przypięty i woluminami warstwowymi (z archiwizacji jest podzbiorem warstwowa). Z drugiej strony może mieć warstwowej urządzenia przed aktualizacją 2 oraz archiwalne woluminów. 

Skorzystaj z poniższej tabeli, aby określić, czy możesz w trybie Failover na inne urządzenie uruchomiona wersja innego oprogramowania i zachowanie typy woluminu podczas odzyskiwania po awarii.

| Tryb failover z | Dozwolone w przypadku urządzenia fizycznego | Dozwolony dla urządzenia wirtualnego |
| --- | --- | --- |
| Aktualizacja 2, aby przed wprowadzeniem aktualizacji Update 1 (wersja, 0.1, 0.2, 0.3) |Nie |Nie |
| Aktualizacja 2, aby zaktualizować 1 (1, 1.1, 1.2) |Tak <br></br>Przy użyciu lokalnie przypięty lub do warstwy woluminów lub zmieszanie dwóch, woluminy są zawsze nie powiodła się nad jak do warstwy. |Tak<br></br>Za pomocą lokalnie przypięty woluminów, te są nie ponad warstwy. |
| Aktualizacja 2 do wersji Update 2 (późniejsza wersja) |Tak<br></br>Jeśli przy użyciu woluminów lokalnie przypiętych lub warstwowych lub mieszane dwa, woluminy są zawsze nie powiodło się za pośrednictwem jako początkowy typ woluminu; warstwowej jako warstwowych i przypięty lokalnie, przypięty lokalnie. |Tak<br></br>Za pomocą lokalnie przypięty woluminów, te są nie ponad warstwy. |

#### <a name="partial-failover-across-software-versions"></a>Częściowe trybu failover między wersjami oprogramowania
Jeśli zamierzasz wykonać częściowe trybu failover przy użyciu urządzenia StorSimple źródła systemem 1 przed aktualizacją do obiektu docelowego z aktualizacji 1 lub nowszą, wykonaj tę wskazówkę. 

| Częściowe przejściu w tryb failover | Dozwolone w przypadku urządzenia fizycznego | Dozwolony dla urządzenia wirtualnego |
| --- | --- | --- |
| Przed wprowadzeniem aktualizacji Update 1 (wersja, 0.1, 0.2, 0.3) Update 1 lub nowszy |Tak, wymienione poniżej najlepsze praktyki poradę. |Tak, wymienione poniżej najlepsze praktyki poradę. |

> [!TIP]
> Wystąpił chmury metadane i dane formatu zmiany w aktualizacji 1 lub nowszy. W związku z tym nie zaleca się częściowe przejściu w tryb failover przed aktualizacją 1 Update 1 lub nowszy. Jeśli zachodzi potrzeba wykonania częściowe trybu failover, zaleca się najpierw zastosować Update 1 lub nowszy na obu urządzeń (źródłowy i docelowy), a następnie kontynuować pracę w trybie failover. 
> 
> 

## <a name="fail-over-to-another-physical-device"></a>Przełączyć do innego urządzenia fizycznego
Wykonaj poniższe kroki, aby przywrócić urządzenia docelowego urządzenia fizycznego.

1. Upewnij się, że kontener woluminów, które chcesz przełączyć skojarzone migawki w chmurze.
2. Na **urządzeń** kliknij przycisk **kontenery woluminów** kartę.
3. Wybierz kontener woluminów, które chcesz przełączyć się na innym urządzeniu. Kliknij przycisk kontenera woluminów, aby wyświetlić listę woluminów, w tym kontenerze. Wybierz wolumin, a następnie kliknij przycisk **Przełącz do trybu Offline** woluminu przełączenia do trybu offline. Powtórz ten proces, wszystkie woluminy w kontenerze woluminów.
4. Powtórz poprzedni krok dla wszystkich kontenerów woluminu, który chcesz przełączyć się na innym urządzeniu.
5. Na **urządzeń** kliknij przycisk **pracy awaryjnej**.
6. W kreatorze, który zostaje otwarty, w obszarze **Wybierz kontener woluminów do pracy awaryjnej**:
   
   1. Na liście kontenery woluminów wybierz kontenery woluminów, które chcesz przełączyć.
      **Wyświetlane są tylko kontenery woluminów z migawki w chmurze skojarzone i woluminy w trybie offline.**
   2. W obszarze **wybierz urządzenie docelowe** woluminów z wybranych kontenerów, wybierz urządzenie docelowe z listy rozwijanej dostępnych urządzeń. Tylko na urządzeniach, które mają dostępnej pojemności są wyświetlane na liście rozwijanej.
   3. Na koniec przejrzeć wszystkie ustawienia trybu failover w **potwierdzić trybu failover**. Kliknij ikonę znacznika wyboru ![Ikona znacznika wyboru](./media/storsimple-device-failover-disaster-recovery/IC740895.png).
7. Utworzono zadanie trybu failover, które można monitorować za pomocą **zadania** strony. Jeśli kontener woluminów, które zakończyły się niepowodzeniem na woluminach lokalnych, zostanie wyświetlona indywidualnych zadań przywracania dla każdego woluminu lokalnego (a nie dla woluminów warstwowych) w kontenerze. Te przywracania zadania może potrwać jakiś czas, aby zakończyć. Istnieje prawdopodobieństwo, że zadanie trybu failover może zakończyć się wcześniej. Należy zauważyć, że woluminy te będą lokalnych gwarancji dopiero po zakończeniu zadania przywracania. Po zakończeniu pracy awaryjnej, przejdź do **urządzeń** strony.                                            
   
   1. Wybierz urządzenie, która została użyta jako urządzenie docelowe dla procesu pracy awaryjnej.
   2. Przejdź do **kontenery woluminów** strony. Wszystkie kontenery woluminów, wraz z woluminami ze starym urządzeniem powinien być wyświetlany.

## <a name="failover-using-a-single-device"></a>Tryb failover przy użyciu jednego urządzenia
Jeśli tylko jednego urządzenia i konieczne jest wykonanie trybu failover, wykonaj następujące kroki.

1. Twórz migawki chmurze wszystkie woluminy w urządzeniu.
2. Resetować urządzenie do ustawień fabrycznych. Postępuj zgodnie z instrukcjami szczegółowe [sposób resetowania urządzenia StorSimple do domyślnych ustawień fabrycznych](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).
3. Konfigurowanie urządzenia i zarejestrować go ponownie przy użyciu usługi Menedżer StorSimple.
4. Na **urządzeń** strony, stare urządzenia powinny być widoczne jako **Offline**. Nowo zarejestrowane urządzenia powinny być widoczne jako **Online**.
5. Dla nowego urządzenia wykonaj najpierw minimalnej konfiguracji urządzenia. 
   
   > [!IMPORTANT]
   > **Minimalna konfiguracja najpierw nie zostało ukończone, programu DR zakończy się niepowodzeniem w wyniku błędu w bieżącej implementacji. Ten problem zostanie rozwiązany w nowszej wersji.**
   > 
   > 
6. Wybierz urządzenie stary (stan w trybie offline), a następnie kliknij przycisk **pracy awaryjnej**. W kreatorze, które są prezentowane w tryb failover to urządzenie i określ urządzenie docelowe jako nowo zarejestrowanym urządzeniu. Aby uzyskać szczegółowe instrukcje, zapoznaj się [przełączyć do innego urządzenia fizycznego](#fail-over-to-another-physical-device).
7. Zadanie przywracania urządzenie zostanie utworzony, można monitorować z **zadania** strony.
8. Po pomyślnym ukończeniu zadania dostępu do nowego urządzenia i przejdź do **kontenery woluminów** strony. Kontenery woluminów z urządzenia starego teraz ma zostać poddana migracji do nowego urządzenia.

## <a name="fail-over-to-a-storsimple-virtual-device"></a>Przełączyć urządzenia wirtualnego StorSimple
Musi mieć StorSimple, urządzenie wirtualne utworzone i skonfigurowane przed uruchomieniem tej procedury. Jeśli z aktualizacją Update 2, rozważ użycie urządzenie wirtualne 8020 dla DR, który ma 64 TB i używa magazynu w warstwie Premium. 

Wykonaj poniższe kroki, aby przywrócić urządzenia docelowego urządzenia wirtualnego StorSimple.

1. Upewnij się, że kontener woluminów, które chcesz przełączyć skojarzone migawki w chmurze.
2. Na **urządzeń** kliknij przycisk **kontenery woluminów** kartę.
3. Wybierz kontener woluminów, które chcesz przełączyć się na innym urządzeniu. Kliknij przycisk kontenera woluminów, aby wyświetlić listę woluminów, w tym kontenerze. Wybierz wolumin, a następnie kliknij przycisk **Przełącz do trybu Offline** woluminu przełączenia do trybu offline. Powtórz ten proces, wszystkie woluminy w kontenerze woluminów.
4. Powtórz poprzedni krok dla wszystkich kontenerów woluminu, który chcesz przełączyć się na innym urządzeniu.
5. Na **urządzeń** kliknij przycisk **pracy awaryjnej**.
6. W kreatorze, który zostaje otwarty, w obszarze **Wybierz kontener woluminów do trybu failover**, wykonaj następujące czynności:
   
    a. Na liście kontenery woluminów wybierz kontenery woluminów, które chcesz przełączyć.
   
    **Wyświetlane są tylko kontenery woluminów z migawki w chmurze skojarzone i woluminy w trybie offline.**
   
    b. W obszarze **wybierz urządzenie docelowe dla woluminów w kontenerach wybranego**, wybierz urządzenie wirtualne StorSimple z listy rozwijanej dostępnych urządzeń. **Tylko na urządzeniach, które mają wystarczającą pojemność są wyświetlane na liście rozwijanej.**  
7. Na koniec przejrzeć wszystkie ustawienia trybu failover w **potwierdzić trybu failover**. Kliknij ikonę znacznika wyboru ![Ikona znacznika wyboru](./media/storsimple-device-failover-disaster-recovery/IC740895.png).
8. Po zakończeniu pracy awaryjnej, przejdź do **urządzeń** strony.
   
    a. Wybierz urządzenie wirtualne StorSimple, która została użyta jako urządzenie docelowe dla procesu pracy awaryjnej.
   
    b. Przejdź do **kontenery woluminów** strony. Teraz powinien być wyświetlany wszystkie kontenery woluminów, wraz z woluminami ze starym urządzeniem.

![Zobacz film](./media/storsimple-device-failover-disaster-recovery/Video_icon.png) **Zobacz film**

Aby obejrzeć film wideo przedstawiający sposób można przywrócić nieudanej przez urządzenie fizyczne do urządzenia wirtualnego w chmurze, kliknij [tutaj](https://azure.microsoft.com/documentation/videos/storsimple-and-disaster-recovery/).

## <a name="failback"></a>Powrót po awarii
Update 3 i nowszych StorSimple obsługuje powrót po awarii. Po zakończeniu pracy awaryjnej są wykonywane następujące akcje:

* Kontenery woluminów, które są przełączenie do trybu failover są czyszczone z urządzenia źródłowego.
* Zadanie w tle na kontenera woluminów (failover) jest inicjowana na urządzenia źródłowego. Przy próbie powrotu po awarii podczas zadania w toku, otrzymasz powiadomienie w tym celu. Należy poczekać, aż zakończeniu zadania, aby uruchomić powrót po awarii. 
  
    Czas wymagany do ukończenia usuwania kontenery woluminów zależy od różnych czynników, takich jak ilość danych, wiek danych, liczba kopii zapasowych i przepustowość sieci dostępną dla tej operacji. Jeśli planujesz praca awaryjna testu powrotu po awarii, zaleca się przetestowanie kontenery woluminów o mniejszej ilości danych (GB). W większości przypadków można uruchomić powrót po awarii 24 godziny po zakończeniu pracy awaryjnej. 

## <a name="frequently-asked-questions"></a>Często zadawane pytania
Q. **Co się stanie, jeśli DR nie powiedzie się lub została częściowe Powodzenie?**

A. W przypadku niepowodzenia DR zaleca się, spróbuj ponownie. Po raz drugi około, odzyskiwania po awarii wie, jaki wszystkie były generowane i podczas procesu zablokowany po raz pierwszy. Proces odzyskiwania po awarii rozpoczyna się od tego momentu i jego nowszych wersjach. 

Q. **Urządzenie można usunąć w trakcie pracy awaryjnej urządzenia?**

A. Nie można usunąć urządzenia w trakcie odzyskiwania po awarii. Urządzenie można usunąć tylko po zakończeniu odzyskiwania po awarii.

Q.    **Gdy wyrzucanie elementów bezużytecznych rozpoczyna się na urządzeniu źródła, aby dane lokalne na urządzenia źródłowego jest usuwane?**

A. Wyrzucanie elementów bezużytecznych zostaną włączone na urządzeniu źródła tylko wtedy, gdy urządzenie jest całkowicie wyczyścić. Oczyszczanie obejmuje czyszczenia obiektów, które nie powiodły się za pośrednictwem z urządzenia źródłowego, takich jak woluminy, liczby obiektów kopii zapasowej (a nie dane), kontenery woluminów i zasady.

Q. **Co się stanie, jeśli zadanie usuwania skojarzone z kontenerów wolumin w urządzeniu źródła zakończy się niepowodzeniem?**

A.  Jeśli zadanie usuwania nie powiedzie się, a następnie trzeba będzie ręcznie uruchomić usunięcie kontenery woluminów. W **urządzeń** wybierz urządzenia źródłowego i kliknij przycisk **kontenery woluminów**. Wybierz kontenery woluminów, których nie powiodła się za pośrednictwem i w dolnej części strony, kliknij przycisk **usunąć**. Po usunięciu wszystkich nieudanych za pośrednictwem kontenery woluminów na urządzeniu źródła, możesz uruchomić powrót po awarii.

## <a name="business-continuity-disaster-recovery-bcdr"></a>Odzyskiwanie po awarii ciągłości biznesowej (BCDR)
Scenariusz odzyskiwania (BCDR) po awarii ciągłości biznesowej występuje, gdy całe centrum danych Azure przestanie działać. To może mieć wpływ na usługi Menedżer StorSimple i skojarzone urządzenia StorSimple.

W przypadku urządzeń StorSimple, które zostały zarejestrowane bezpośrednio przed zamknięciem wystąpił awarii, następnie te urządzenia StorSimple może być konieczne przeprowadzenie resetowania do ustawień fabrycznych. Po awarii urządzenia StorSimple będą wyświetlane w trybie offline. Urządzenia StorSimple muszą zostać usunięte z portalu i resetowania do ustawień fabrycznych należy zrobić, następuje świeże rejestracji.

## <a name="next-steps"></a>Następne kroki
* Po wykonaniu trybu failover, konieczne może być [dezaktywację lub usunięcie urządzenia StorSimple](storsimple-deactivate-and-delete-device.md).
* Aby uzyskać informacje o sposobie używania usługi Menedżer StorSimple, przejdź do [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md).

