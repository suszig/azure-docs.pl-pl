---
title: "Zarządzanie szablonami przepustowości StorSimple | Dokumentacja firmy Microsoft"
description: "Opisuje sposób zarządzania StorSimple przepustowości szablonów, które umożliwiają kontrolowanie użycia przepustowości."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 0027b90e-91a5-437d-9bd0-06b05674aa5f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2016
ms.author: alkohli
ms.openlocfilehash: df3ae8bf775370432b3648459a7c942afe69fb17
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-storsimple-bandwidth-templates"></a>Zarządzaj szablonami przepustowości StorSimple przy użyciu usługi Menedżer StorSimple
## <a name="overview"></a>Omówienie
Szablony przepustowości umożliwiają konfigurowanie użycia przepustowości sieci między kilka harmonogramów czasu dnia do warstwy danych z urządzenia StorSimple w chmurze.

Harmonogramy ograniczania przepustowości można:

* Określ harmonogramy niestandardowych przepustowości w zależności od użycia sieci obciążenia.
* Scentralizowane zarządzanie i ponowne użycie harmonogramy na wielu urządzeniach w sposób łatwy i bezproblemowe.

> [!NOTE]
> Ta funkcja jest dostępna tylko w przypadku urządzenia fizycznego StorSimple, a nie dla urządzenia wirtualnego.
> 
> 

Wszystkie szablony przepustowości usługi są wyświetlane w formacie tabelarycznym i zawiera następujące informacje:

* **Nazwa** — nazwa przypisana do szablonu przepustowości podczas jej tworzenia.
* **Harmonogram** — liczba harmonogramów zawarte w szablonie danego przepustowości.
* **Używane przez** — liczba woluminów za pomocą szablonów przepustowości.

Użyj usługi Menedżer StorSimple **Konfiguruj** strony w klasycznym portalu Azure w celu zarządzania szablonami przepustowości.

Można również znaleźć dodatkowe informacje ułatwiające konfigurowanie szablonów przepustowości w:

* Pytania i odpowiedzi dotyczące przepustowości szablonów
* Najlepsze rozwiązania dotyczące przepustowości szablonów

## <a name="add-a-bandwidth-template"></a>Dodaj szablon przepustowości
Wykonaj poniższe kroki, aby utworzyć nowy szablon przepustowości.

#### <a name="to-add-a-bandwidth-template"></a>Aby dodać szablon przepustowości
1. W usłudze Menedżer StorSimple **Konfiguruj** kliknij przycisk **Dodaj/Edytuj szablon przepustowości**.
2. W **Dodawanie/edytowanie szablonu przepustowości** okno dialogowe:
   
   1. Z **szablonu** listy rozwijanej wybierz **Utwórz nowy** Aby dodać nowy szablon przepustowości.
   2. Określ unikatową nazwę dla szablonu przepustowości.
3. Zdefiniuj **przepustowości, harmonogram**. Aby utworzyć harmonogram:
   
   1. Z listy rozwijanej wybierz dni tygodnia harmonogramu jest skonfigurowany dla. Zaznaczając pole wyboru znajdujące się przed upływem odpowiednich dni na liście, można wybrać kilka dni.
   2. Wybierz **cały dzień** opcji w przypadku, gdy harmonogram jest wymuszana przez cały dzień. Po zaznaczeniu tej opcji nie będzie można określić **czas rozpoczęcia** lub **czas zakończenia**. Harmonogram uruchamiania od 00:00:00 do 11:59 PM.
   3. Z listy rozwijanej wybierz **czas rozpoczęcia**. Jest to, gdy rozpocznie się harmonogramu.
   4. Z listy rozwijanej wybierz **czas zakończenia**. Jest to harmonogram zostanie zatrzymane.
      
      > [!NOTE]
      > Nakładające się harmonogramy nie są dozwolone. Czas rozpoczęcia i zakończenia spowoduje nakładające się harmonogramu, pojawi się odpowiedni komunikat o błędzie.
      > 
      > 
   5. Określ **szybkość przepustowości**. Jest to przepustowość w megabitach na sekundę (MB/s) używany przez urządzenia StorSimple w operacji dotyczących chmury (przekazywania i pobierania). Wprowadź liczbę z zakresu od 1 do 1000 dla tego pola.
   6. Kliknij ikonę znacznika wyboru ![Ikona znacznika wyboru](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Szablon, który został utworzony zostanie dodany do listy szablonów przepustowości w usłudze **Konfiguruj** strony.
      
      ![Utwórz nowy szablon przepustowości](./media/storsimple-manage-bandwidth-templates/HCS_CreateNewBT1.png)
4. Kliknij przycisk **zapisać** w dolnej części strony, a następnie kliknij przycisk **tak** po wyświetleniu monitu o potwierdzenie. Spowoduje to zapisanie zmian konfiguracji, które zostały wprowadzone.

## <a name="edit-a-bandwidth-template"></a>Edytuj szablon przepustowości
Wykonaj poniższe kroki, aby edytować szablon przepustowości.

### <a name="to-edit-a-bandwidth-template"></a>Aby edytować szablon przepustowości
1. Kliknij przycisk **Dodaj/Edytuj szablon przepustowości**.
2. W **Dodawanie/edytowanie szablonu przepustowości** okno dialogowe:
   
   1. Z **szablonu** listy rozwijanej wybierz istniejący szablon przepustowości, który chcesz zmodyfikować.
   2. Wprowadź zmiany. (Można zmodyfikować wszystkich istniejących ustawień.)
   3. Kliknij ikonę znacznika wyboru ![Ikona znacznika wyboru](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Na stronie Konfiguracja usługi zobaczysz zmodyfikowanego szablonu na liście szablonów przepustowości.
3. Aby zapisać zmiany, kliknij przycisk **zapisać** w dolnej części strony. Kliknij przycisk **tak** po wyświetleniu monitu o potwierdzenie.

> [!NOTE]
> Będzie nie można zapisać zmiany, jeśli edytowanych harmonogramu pokrywa się z istniejącego harmonogramu modyfikowanego szablonu przepustowości.
> 
> 

## <a name="delete-a-bandwidth-template"></a>Usuń szablon przepustowości
Wykonaj poniższe kroki, aby usunąć szablon przepustowości.

#### <a name="to-delete-a-bandwidth-template"></a>Aby usunąć szablon przepustowości
1. W tabelarycznej listę szablonów przepustowości dla usługi wybierz szablon, który chcesz usunąć. Ikona usuwania (**x**) pojawi się skrajne po prawej stronie wybranego szablonu. Kliknij przycisk **x** ikonę, aby usunąć szablon.
2. Pojawi się monit o potwierdzenie. Kliknij przycisk **OK** aby kontynuować.

Jeśli szablon jest używany przez wszystkie woluminy, nie będzie można go usunąć. Pojawi się komunikat o błędzie wskazujący, że szablon jest używany. Zostanie wyświetlone okno dialogowe komunikat błędu, udzielanie porad należy usunąć wszystkie odwołania do szablonu.

Możesz usunąć wszystkie odwołania do szablonu po zalogowaniu się do **kontenery woluminów** strony i modyfikowanie kontenery woluminów, które użyć tego szablonu, aby użyć innego szablonu lub użyć ustawienia niestandardowe lub nieograniczone przepustowości. Po usunięciu wszystkich odwołań, można usunąć szablonu.

## <a name="use-a-default-bandwidth-template"></a>Użyj domyślnego szablonu przepustowości
Domyślny szablon przepustowości podano i jest używany przez kontenery woluminów domyślnie w celu wymuszenia kontroli przepustowości podczas uzyskiwania dostępu do chmury. Domyślny szablon służy również jako punkt odniesienia gotowy dla użytkowników, którzy tworzyć własne szablony. Szczegóły tego szablonu domyślnego są:

* **Nazwa** — w nocy nieograniczone, jak i w weekendy
* **Harmonogram** — jeden harmonogram od poniedziałku do piątku stosowanym współczynnika przepustowości 1 MB/s miedzy 8 a 17: 00 czasu urządzenia. W pozostałej części tygodnia wynosi przepustowości bez ograniczeń.

Można edytować szablon domyślny. Użycie tego szablonu (w tym wersje edytowanych) są śledzone.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Tworzenie szablonu przepustowości całodzienne, która rozpoczyna się od określonego czasu
Wykonaj tę procedurę, aby utworzyć harmonogram, który rozpoczyna się od określonego czasu i uruchamia cały dzień. W tym przykładzie harmonogram rozpoczyna się od 9 AM w nocy i uruchamia do 9 AM następnego dnia rano. Należy pamiętać, że godziny rozpoczęcia i zakończenia dla danego harmonogramu muszą być jednocześnie zawarte na ten sam harmonogram 24-godzinnym i nie może obejmować wiele dni. Jeśli trzeba zdefiniować szablony przepustowości, obejmujące wiele dni, należy użyć wielu harmonogramów (jak pokazano w przykładzie).

#### <a name="to-create-an-all-day-bandwidth-template"></a>Aby utworzyć szablon przepustowości całodzienne
1. Utwórz harmonogram, który rozpoczyna się od 9 AM w nocy i uruchamia do północy.
2. Dodaj inny harmonogram. Skonfiguruj drugi harmonogram wykonywania od północy do 9 AM w nocy.
3. Zapisz szablon przepustowości.

Harmonogram złożony będą uruchomione w czasie wybrane i uruchom całodzienne.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Pytania i odpowiedzi dotyczące przepustowości szablonów
**Q**. Co się dzieje kontroli przepustowości podczas pracy Between harmonogramy? (Zakończył się zgodnie z harmonogramem i innego nie została jeszcze uruchomiona.)

**A**. W takich przypadkach zostanie zastosowane bez kontroli przepustowości. Oznacza to, że urządzenie może używać nieograniczone przepustowości podczas tworzenia warstw danych w chmurze.

**Q**. Czy możesz zmodyfikować szablony przepustowość na urządzeniu w trybie offline?

**A**. Nie można zmodyfikować szablony przepustowość na kontenery woluminów, jeśli odpowiednie urządzenie jest w trybie offline.

**Q**. Możesz edytować szablon przepustowości skojarzone z kontenera woluminów, gdy skojarzone woluminy są w trybie offline

**A**. Można zmodyfikować szablonu przepustowości skojarzone z kontenera woluminów, których woluminy są w trybie offline. Należy pamiętać, że gdy woluminy są w trybie offline, żadne dane nie będą należeć do warstwy z urządzenia do chmury.

**Q**. Można usunąć domyślny szablon?

**A**. Mimo że można usunąć szablonu domyślnego, nie jest dobrym rozwiązaniem, aby to zrobić. Użycie szablonu domyślnego, tym edytowanych wersje są śledzone. Dane śledzenia są analizowane i w ciągu godziny jest używany do zwiększenia domyślnego szablonu.

**Q**. Jaki sposób można określić, czy szablony przepustowości muszą zostać zmodyfikowane?

**A**. Jest jednym ze znaków, należy zmodyfikować szablony przepustowości podczas uruchamiania wyświetlać sieci działa wolno lub podlewka wiele razy w ciągu jednego dnia. W takim przypadku monitorowania sieci magazynu i użycia, analizując wykresy wydajności operacji We/Wy i przepustowość sieci.

Z danych przepustowość sieci należy zidentyfikować godzina i kontenery woluminów, w których występuje wąskich gardeł. Jeśli dzieje, gdy jest on warstwowa danych do chmury (uzyskać te informacje z wydajność We/Wy dla wszystkich kontenerów woluminu dla urządzenia do chmury), a następnie należy zmodyfikować szablony przepustowości skojarzone z kontenerów woluminu.

Po modyfikacji szablony są używane, należy monitorować ponownie znaczne opóźnienia sieci. Jeśli te nadal istnieje, następnie konieczne będzie ponowne przeanalizowanie szablonów przepustowości.

**Q**. Co się stanie, jeśli ma wiele kontenerów wolumin w urządzeniu planuje tego nakładają się na siebie, ale różne limity mają zastosowanie do każdej?

**A**. Załóżmy, że masz urządzenie z 3 kontenery woluminów. Harmonogramy skojarzone z tych kontenerów całkowicie nakładają się. Dla każdego z tych kontenerów limity przepustowości, używane są 5, 10 lub 15 MB/s odpowiednio. Podczas operacji We/Wy występujących w wszystkie kontenery w tym samym czasie, co najmniej 3 limity przepustowości mogą być stosowane: w tym przypadku 5 MB/s one wychodzących żądań We/Wy udostępnić tę samą kolejkę.

## <a name="best-practices-for-bandwidth-templates"></a>Najlepsze rozwiązania dotyczące przepustowości szablonów
Należy stosować następujące najlepsze rozwiązania dla urządzenia StorSimple:

* Konfigurowanie szablonów przepustowość na urządzeniu, aby włączyć zmiennej ograniczania przepustowości sieci przez urządzenie o różnych porach dnia. Te szablony przepustowości, gdy jest używany z harmonogramy tworzenia kopii zapasowej można efektywnie wykorzystać dodatkowej przepustowości sieci dla operacji poza godzinami szczytu.
* Oblicz rzeczywistej wymaganej dla konkretnego wdrożenia na podstawie rozmiaru wdrożenia i celu czasu wymaganego odzyskiwania (RTO) przepustowości.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [przy użyciu usługi Menedżer StorSimple do administrowania urządzeniem StorSimple](storsimple-manager-service-administration.md).

