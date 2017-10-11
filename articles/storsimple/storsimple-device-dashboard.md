---
title: "Pulpit nawigacyjny urządzeń Menedżer StorSimple | Dokumentacja firmy Microsoft"
description: "Opisuje pulpit nawigacyjny urządzenia usługi Menedżer StorSimple i jak z niego korzystać, aby wyświetlić metryki magazynu i inicjatorów połączonych i znaleźć numer seryjny i IQN."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 6c213969-a385-461f-b698-78ef5b8a79cc
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0d8035b9608ca3bac3d4822c7c755b81c96d481e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-device-dashboard-in-storsimple-manager-service"></a>Pulpit nawigacyjny urządzenia w usłudze Menedżer StorSimple  

## <a name="overview"></a>Omówienie
Pulpit nawigacyjny urządzenia StorSimple Manager zapewnia przegląd informacji dla określonego urządzenia StorSimple, w przeciwieństwie do pulpitu nawigacyjnego usługi, który zawiera informacje o wszystkich urządzeniach zawarte w rozwiązaniu Microsoft Azure StorSimple.

![Strona pulpitu nawigacyjnego urządzenia](./media/storsimple-device-dashboard/StorSimple_DeviceDashbaord1M.png)

Pulpit nawigacyjny zawiera następujące informacje:

* **Obszar wykresu** — widać metryki istotne magazynu w obszarze wykresu w górnej części pulpitu nawigacyjnego. Na tym wykresie można wyświetlić metryki całkowita ilość miejsca głównej (ilość danych zapisanych przez hosty z urządzeniem) i magazynu w chmurze całkowita liczba zużywane przez urządzenie w danym okresie czasu.
  
     W tym kontekście *magazynu głównego* odwołuje się do całkowitej ilości danych zapisanych przez hosta i mogą być podzielone przez typ woluminu: *podstawowego do warstwy magazynu* obejmuje zarówno lokalnie przechowywanych danych i warstwy do Chmura; *głównej przypięty lokalnie magazynu* zawiera tylko dane przechowywane lokalnie. *Magazyn w chmurze*, z drugiej strony, jest wartość całkowita ilość danych przechowywanych w chmurze. W tym warstwowych danych i tworzenie kopii zapasowych. Zauważ, że dane przechowywane w chmurze jest deduplikowany i kompresji magazynu podstawowego wskazuje ilość miejsca w magazynie używana, zanim dane jest deduplikowany i skompresowane. (Porównanie tych dwóch liczb, aby poznać częstotliwość kompresji). Dla obu lokacji podstawowej i magazynu w chmurze, wartości podanych w oparciu częstotliwość śledzenia, należy skonfigurować. Na przykład jeśli częstotliwość tydzień, następnie wykresu spowodują wyświetlenie danych dla poszczególnych dni w poprzednim tygodniu.
  
     Wykres można skonfigurować w następujący sposób:
  
  * Aby sprawdzić ilość magazynu w chmurze używane wraz z upływem czasu, wybierz **używany Magazyn w CHMURZE** opcji. Aby wyświetlić całkowita ilość miejsca, które zostały napisane przez hosta, wybierz **podstawowego do warstwy MAGAZYNU używane** i **głównej LOKALNIE PRZYPIĘTY Magazyn używany** opcje. Na ilustracji są zaznaczone obie opcje; w związku z tym na wykresie przedstawiono ilości pamięci masowej dla chmury i podstawowego magazynu. Należy pamiętać, że każdy magazyn podstawowy używany przed zainstalowaniem aktualizacji 2 jest reprezentowana przez **podstawowego do warstwy MAGAZYNU używane** wiersza.
  * Użyj menu rozwijanego w prawym górnym rogu wykresu, aby określić okres czasu 1 tydzień, 1-miesięcznego, 3-miesięczna lub 1 rok. Zauważ, że wykres najwyższego poziomu zostanie odświeżony tylko jeden raz dziennie i w związku z tym będzie odzwierciedlać sumy poprzedniego dnia.
    
    Aby uzyskać więcej informacji, zobacz [monitorować urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-monitor-device.md).
* **Przegląd wykorzystania** — w **przegląd wykorzystania** obszaru, można obejrzeć ilość miejsca w magazynie podstawowy używany, ilość zainicjowanego magazynu i pojemności pamięci masowej dla danego urządzenia. Porównując te liczby użycia maksymalną ilość pamięci, która jest dostępna, można wyświetlić w skrócie Uzyskaj dodatkowe miejsce do magazynowania. Warto zauważyć, że w tym omówieniu jest aktualizowany co 15 minut, z powodu różnicy w częstotliwość aktualizacji mogą być wyświetlane różne numery niż te wyświetlane w obszarze wykresu powyżej, która jest aktualizowana raz dziennie. Aby uzyskać więcej informacji, zobacz [monitorować urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-monitor-device.md).
* **Alerty** — **alerty** obszaru zawiera omówienie alerty dla danego urządzenia. Alerty są pogrupowane według ważności, a liczby alertów na każdym poziomie ważność podana jest liczba. Ważność alertu kliknięcie spowoduje otwarcie widoku zakresami kartę alerty, aby pokazać tylko alerty ten poziom ważności, dla tego urządzenia.
* **Zadania** — **zadania** obszaru przedstawia wynik ostatniej aktywności zadania. To należy zapewnić system działa zgodnie z oczekiwaniami, czy można pozwalają wiedzieć, że trzeba podjąć działania naprawcze. Aby uzyskać więcej informacji o ostatnio wykonanych zadań, kliknij przycisk **zadań zakończyło się pomyślnie w ciągu ostatnich 24 godzin**.
* **Szybkiego dostępu** obszaru po prawej stronie pulpitu nawigacyjnego zawiera przydatne informacje, takie jak model urządzenia, numer seryjny, stan, opisu i liczby woluminów.

Można również skonfigurować trybu failover i wyświetlić inicjatorów połączonych z poziomu pulpitu nawigacyjnego urządzenia.

Typowe zadania, które mogą być wykonywane na tej stronie są:

* Wyświetl połączone inicjatorów
* Znaleźć numer seryjny urządzenia
* Znajdź urządzenia docelowego IQN

## <a name="view-connected-initiators"></a>Wyświetl połączone inicjatorów
Możesz wyświetlić inicjatorów iSCSI, które są podłączone do urządzenia, klikając **wyświetlanie połączonych inicjatorów** łącza w **szybkiego dostępu** części pulpitu nawigacyjnego urządzenia. Ta strona zawiera listę tabelarycznym inicjatory, względem których pomyślnie nawiązano połączenie z urządzeniem. Dla każdego inicjatora można wyświetlić:

* ISCSI kwalifikowana nazwa (IQN) podłączonej inicjatora.
* Nazwa rekordu kontroli dostępu (ACR), który umożliwia tego inicjatora połączonych.
* Adres IP połączenia inicjatora.
* Interfejsy sieciowe podłączone do inicjatora urządzenia magazynującego. Te mogą należeć do zakresu od dane 0 do dane 5.
* Wszystkie woluminy, które połączonych inicjatora może dostępu zgodnie z bieżącą konfigurację ACR.

Jeśli Zobacz nieoczekiwany inicjatorów na tej liście lub nie ma oczekiwanymi, Przejrzyj konfigurację ACR. Maksymalnie 512 inicjatorów mogą łączyć się urządzenia.

## <a name="find-the-device-serial-number"></a>Znaleźć numer seryjny urządzenia
Numer seryjny urządzenia może być konieczne po skonfigurowaniu wielościeżkowego We/Wy (MPIO) firmy Microsoft na urządzeniu. Wykonaj poniższe kroki, aby znaleźć numer seryjny urządzenia.

#### <a name="to-find-the-device-serial-number"></a>Aby znaleźć numer seryjny urządzenia
1. Przejdź do **urządzeń** > **pulpitu nawigacyjnego**.
2. W okienku po prawej stronie pulpitu nawigacyjnego, Znajdź **szybkiego dostępu** obszaru.
3. Przewiń w dół, a następnie zlokalizuj numer seryjny.

## <a name="find-the-device-target-iqn"></a>Znajdź urządzenia docelowego IQN
Element docelowy urządzenia IQN może być konieczne podczas konfigurowania protokołu uwierzytelniania typu Challenge Handshake (CHAP) na urządzeniu StorSimple. Wykonaj poniższe kroki, aby znaleźć elementu docelowego urządzenia IQN.

### <a name="to-find-the-device-target-iqn"></a>Aby znaleźć elementu docelowego urządzenia IQN
1. Przejdź do **urządzeń** > **pulpitu nawigacyjnego**.
2. W okienku po prawej stronie pulpitu nawigacyjnego, Znajdź **szybkiego dostępu** obszaru.
3. Przewiń w dół, a następnie zlokalizuj element docelowy IQN.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [pulpit nawigacyjny usługi Menedżer StorSimple](storsimple-service-dashboard.md).
* Dowiedz się więcej o [przy użyciu usługi Menedżer StorSimple do administrowania urządzeniem StorSimple](storsimple-manager-service-administration.md).

