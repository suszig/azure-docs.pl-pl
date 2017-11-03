---
title: "Zarządzanie woluminy StorSimple | Dokumentacja firmy Microsoft"
description: "Wyjaśniono, jak dodawać, modyfikować, monitorowania i Usuń woluminy StorSimple i sposób wykonania ich w trybie offline, jeśli to konieczne."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ccabd859-590c-4568-a81d-6ff38f125be3
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/11/2016
ms.author: v-sharos
ms.openlocfilehash: 31ed9dad8ba56a3746873b7b35e678e97743fbfe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-volumes"></a>Usługę Menedżer StorSimple umożliwia zarządzanie woluminami
[!INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Omówienie
W tym samouczku opisano sposób korzystania z usługi Menedżer StorSimple, tworzenie i zarządzanie nimi woluminów na urządzeniu StorSimple oraz urządzenia wirtualnego StorSimple.

Usługę Menedżer StorSimple jest rozszerzeniem klasycznego portalu Azure, która umożliwia zarządzanie rozwiązania StorSimple z interfejsem sieci web jednej. Oprócz zarządzania woluminami, można użyć usługi Menedżer StorSimple do tworzenia i zarządzania usługi StorSimple, wyświetlanie i zarządzanie urządzeniami, wyświetlanie alertów i wyświetlić i zarządzanie zasadami tworzenia kopii zapasowej i kopii zapasowej wykazu.

> [!NOTE]
> Azure StorSimple można tworzyć tylko woluminy alokowane elastycznie. Nie można utworzyć w pełni lub częściowo inicjowanych woluminów w systemie Azure StorSimple.
> 
> Alokowanie elastyczne jest technologii wirtualizacji, dostępny magazyn prawdopodobnie przekracza zasobów fizycznych. Zamiast z wyprzedzeniem rezerwowania wystarczającej ilości miejsca, Azure StorSimple używa alokowania elastycznego można przydzielić tylko za mało miejsca, aby spełniały bieżące wymagania. Elastyczne rodzaj magazynu w chmurze ułatwia takie podejście, ponieważ Azure StorSimple można zwiększyć lub zmniejszyć do zmieniających się wymagań dotyczących magazynu w chmurze.
> 
> 

## <a name="the-volumes-page"></a>Strony woluminów
**Woluminów** strona umożliwia zarządzanie woluminami magazynu, które są udostępniane na urządzeniu Microsoft Azure StorSimple na potrzeby Twojego inicjatorów (serwerów). Wyświetli listę woluminów na urządzeniu StorSimple.

 ![Strony woluminów](./media/storsimple-manage-volumes/HCS_VolumesPage.png)

Wolumin składa się z szeregu atrybuty:

* **Nazwa** — opisową nazwę, która musi być unikatowa i pomaga w identyfikacji woluminu. Ta nazwa jest również używana do monitorowania raportów podczas filtrowania w określonym woluminie.
* **Stan** — może być w trybie online lub offline. Jeśli wolumin, jeśli jest w trybie offline, nie jest widoczny dla inicjatorów (serwerów), które mają dostęp do korzystać z woluminu.
* **Pojemność** — Określa, jak duży woluminu, jak postrzegane przez inicjatora (serwer). Pojemność określa łączną ilość danych, które mogą być przechowywane przez inicjatora (serwer). Woluminy alokowane są udostępniane, a następnie jest deduplikowany danych. Oznacza to, że urządzenie nie wstępnie przydzielić pojemności magazynu fizycznego, wewnętrznych lub w chmurze zgodnie ze skonfigurowanym pojemność. Pojemność woluminu jest przydzielone i używane na żądanie.
* **Typ** — typ woluminu może być warstwowych lub archiwizacji (podtyp o warstwowa)
* **Dostęp** — określa inicjatorów (serwerów), które mają prawo dostępu do tego woluminu. Inicjatory, względem których nie są elementami członkowskimi rekord kontroli dostępu (ACR), który jest skojarzony z woluminem nie będą widzieć woluminu.
* **Monitorowanie** — Określa, czy jest monitorowany wolumin. Wolumin ma monitorowania włączona domyślnie po jego utworzeniu. Monitorowanie będzie, jednak, można wyłączyć dla Klonowanie woluminów. Aby włączyć monitorowanie dla woluminu, postępuj zgodnie z instrukcjami w monitorze woluminu.

Najbardziej typowe zadania związane z woluminem są:

* Dodawanie woluminu
* Modyfikowanie woluminu
* Usuwanie woluminu
* Przełącz do trybu offline woluminu
* Monitor woluminu

## <a name="add-a-volume"></a>Dodawanie woluminu
Możesz [utworzony wolumin](storsimple-deployment-walkthrough-u1.md#step-6-create-a-volume) podczas wdrażania rozwiązania StorSimple. Dodawanie woluminu jest podobnej procedury.

### <a name="to-add-a-volume"></a>Aby dodać wolumin
1. Na **urządzeń** , wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij przycisk **kontenery woluminów** kartę.
2. Wybierz kontener woluminów, a następnie kliknij strzałkę w odpowiednim uzyskać dostęp do woluminów skojarzone z kontenera.
3. Kliknij przycisk **Dodaj** w dolnej części strony. Dodaj uruchamia Kreatora woluminów.
   
     ![Kreator dodawania woluminem ustawienia podstawowe](./media/storsimple-manage-volumes/AddVolume1.png)
4. W Kreatorze dodawania woluminu w obszarze **Ustawienia podstawowe** wykonaj następujące czynności:
   
   1. Wypełnij pole **Nazwa** dla woluminu.
   2. Określ wartość w GB lub TB w polu **Alokowana pojemność**. Pojemność musi wynosić od 1 GB do 64 TB dla urządzenia fizycznego. Maksymalna pojemność, które można udostępnić dla woluminu na urządzeniu wirtualnym StorSimple to 30 TB.
   3. Wybierz **typ użycia** dla woluminu. Jeśli używasz woluminu warstwowego na potrzeby danych archiwalnych, wybranie opcji **Użyj tego woluminu w przypadku rzadziej używanych danych archiwalnych** pola wyboru zmienia rozmiar fragmentu deduplikacji dla woluminu na 512 KB. Jeśli nie zaznaczysz tej opcji, odpowiedni wolumin warstwowy użyje rozmiaru fragmentu wynoszącego 64 KB. Większy rozmiar fragmentu deduplikacji umożliwia urządzeniu usprawnienie transferu dużej ilości danych archiwalnych w chmurze. (Woluminy warstwowe były dawniej nazywane woluminami głównymi.)
   4. Kliknij ikonę strzałki ![ikonę strzałki](./media/storsimple-manage-volumes/HCS_ArrowIcon.png)można przejść do **dodatkowe ustawienia** strony.
      
        ![Dodaj dodatkowe ustawienia kreatora woluminu](./media/storsimple-manage-volumes/AddVolume2.png)
5. W obszarze **dodatkowe ustawienia**, Dodaj nowy rekord kontroli dostępu (ACR):
   
   1. Wybierz rekord kontroli dostępu (ACR) z listy rozwijanej. Alternatywnie możesz dodać nowe ACR. ACRs ustalić, które hosty mogą uzyskiwać dostęp do woluminów dopasowując nazwy IQN hosta z wymienionego w rekordzie.
   2. Zaleca się włączenie domyślnego tworzenia kopii zapasowej przez zaznaczenie pola wyboru **Włącz domyślne tworzenie kopii zapasowej dla tego woluminu**.
   3. Kliknij ikonę znacznika wyboru ![Ikona znacznika wyboru](./media/storsimple-manage-volumes/HCS_CheckIcon.png) Aby utworzyć wolumin z określonymi ustawieniami.

Nowy wolumin jest teraz gotowa do użycia.

## <a name="modify-a-volume"></a>Modyfikowanie woluminu
Zmodyfikuj woluminu, gdy trzeba go rozwinąć lub zmienić hosty, które uzyskują dostęp do woluminu.

> [!IMPORTANT]
> * Jeśli zmodyfikujesz rozmiar woluminu na urządzeniu, rozmiar woluminu musi zostać zmienione na hoście, a także.
> * Są opisane tutaj kroki po stronie hosta dla systemu Windows Server 2012 (2012 R2). Procedury dla systemu Linux lub z innymi systemami operacyjnymi hosta może się różnić. Zapoznaj się z instrukcjami podanymi przez system operacyjny hosta, modyfikując na woluminie z innym systemem operacyjnym hosta.
> 
> 

### <a name="to-modify-a-volume"></a>Aby zmodyfikować woluminu
1. Na **urządzeń** , wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij przycisk **kontenera woluminów** kartę. Ta strona zawiera listę w formacie tabelarycznym wszystkie kontenery woluminów, które są skojarzone z danym urządzeniem.
2. Wybierz kontener woluminów i kliknij go, aby wyświetlić listę woluminów, w tym kontenerze.
3. Na **woluminów** , wybierz wolumin i kliknij przycisk **Modyfikuj**.
4. W Kreatorze modyfikowania woluminu w obszarze **podstawowe ustawienia**, można wykonaj następujące czynności:
   
   * Edytuj **nazwa** i **typu** Jeśli chcesz zmodyfikować wolumin warstwowy do woluminu archiwizacji, wybierając **Użyj tego woluminu w przypadku rzadziej używanych danych archiwalnych** pole wyboru Aby zmienić rozmiar fragmentu deduplikacji dla woluminu na 512 KB.
   * Zwiększ **elastycznie pojemności**. **Alokowana pojemność** zwiększyć. Nie można zmniejszyć wolumin, po jego utworzeniu.
     
     > [!NOTE]
     > Nie można zmienić kontener woluminów jest przypisany do woluminu.
     > 
     > 
5. W obszarze **dodatkowe ustawienia**, można wykonaj następujące czynności:
   
   * Zmodyfikuj ACRs, pod warunkiem, że wolumin jest w trybie offline. Jeśli wolumin jest w trybie online, należy przejść do trybu offline najpierw. Skorzystaj z procedury opisanej w [Przełącz do trybu offline wolumin](#take-a-volume-offline) przed modyfikowanie ACR.
   * Zmodyfikuj listę ACRs po wolumin jest w trybie offline.
     
     > [!NOTE]
     > Nie można zmienić **włączyć domyślnego tworzenia kopii zapasowej dla tego woluminu** opcji dla woluminu.
     > 
     > 
6. Zapisz zmiany, klikając ikonę znacznika wyboru ![ikona znacznika wyboru](./media/storsimple-manage-volumes/HCS_CheckIcon.png). Klasyczny portal Azure wyświetli komunikat aktualizowania woluminu. Wyświetli komunikat informujący gdy wolumin został pomyślnie zaktualizowany.
7. Jeśli rozszerzasz woluminu, należy wykonać następujące czynności na komputerze hosta z systemem Windows:
   
   1. Przejdź do **Zarządzanie komputerem** ->**dysku zarządzania**.
   2. Kliknij prawym przyciskiem myszy **Zarządzanie dyskami** i wybierz **Skanuj dyski**.
   3. Na liście dysków, wybierz wolumin, który zostanie zaktualizowany, kliknij prawym przyciskiem myszy, a następnie wybierz **Rozszerz wolumin**. Zostanie uruchomiony Kreator rozszerzania woluminów. Kliknij przycisk **Dalej**.
   4. Ukończ pracę kreatora, akceptując wartości domyślne. Po zakończeniu pracy Kreatora woluminu powinien być wyświetlony zwiększenie wielkości.

![Zobacz film](./media/storsimple-manage-volumes/Video_icon.png) **Zobacz film**

Aby obejrzeć film wideo przedstawiający sposób Rozszerz wolumin, kliknij przycisk [tutaj](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="take-a-volume-offline"></a>Przełącz do trybu offline woluminu
Konieczne może być Przełącz wolumin do trybu offline podczas planowania go zmodyfikować lub usunąć. Gdy wolumin jest w trybie offline, nie jest dostępny do odczytu i zapisu. Należy podjąć woluminu w trybie offline na hoście, a także na urządzeniu. Wykonaj poniższe kroki, aby przełączyć do trybu offline wolumin.

### <a name="to-take-a-volume-offline"></a>Wolumin przełączenia do trybu offline
1. Upewnij się, że w danym woluminie nie jest używany przed przełączeniem do trybu offline.
2. Zająć pierwszy woluminu w trybie offline na hoście. Eliminuje to potencjalne ryzyko uszkodzeniem danych na woluminie. Aby poznać konkretne kroki zapoznaj się z instrukcjami dla systemu operacyjnego hosta.
3. Gdy host jest w trybie offline, należy wykonać woluminu na urządzeniu w trybie offline, wykonując następujące czynności:
   
   1. Na **urządzeń** , wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij przycisk **kontenery woluminów** kartę. **Kontenery woluminów** karcie wyświetlane w formacie tabelarycznym wszystkie kontenery woluminów, które są skojarzone z danym urządzeniem.
   2. Wybierz kontener woluminów i kliknij go, aby wyświetlić listę woluminów, w tym kontenerze.
   3. Wybierz wolumin, a następnie kliknij przycisk **przełączyć do trybu offline**.
   4. Po wyświetleniu monitu o potwierdzenie kliknij przycisk **Tak**. Wolumin powinno być teraz w trybie offline.
      
      Gdy wolumin jest w trybie offline, **przejdź do trybu Online** opcja staje się dostępna.

> [!NOTE]
> **Przełącz do trybu Offline** polecenie wysyła żądanie do urządzenia woluminu przełączenia do trybu offline. Jeśli hosty w dalszym ciągu używają woluminu, powoduje to zerwane połączenia, ale przełączania w tryb offline wolumin nie powiedzie się.
> 
> 

## <a name="delete-a-volume"></a>Usuwanie woluminu
> [!IMPORTANT]
> Wolumin można usunąć tylko wtedy, gdy jest w trybie offline.
> 
> 

Wykonaj poniższe kroki, aby usunąć wolumin.

### <a name="to-delete-a-volume"></a>Aby usunąć wolumin
1. Na **urządzeń** , wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij przycisk **kontenery woluminów** kartę.
2. Wybierz kontener wolumin zawierający wolumin, który chcesz usunąć. Kliknij przycisk kontenera woluminów, aby uzyskać dostęp do **woluminów** strony.
3. Wszystkie woluminy, które są skojarzone z tym kontenerem są wyświetlane w formacie tabelarycznym. Sprawdź stan woluminu, który chcesz usunąć. Jeśli wolumin do usunięcia nie jest w trybie offline, przejść do trybu offline należy wykonać kroki opisane w [Przełącz do trybu offline wolumin](#take-a-volume-offline).
4. Gdy wolumin jest w trybie offline, kliknij przycisk **usunąć** w dolnej części strony.
5. Po wyświetleniu monitu o potwierdzenie kliknij przycisk **Tak**. Wolumin zostanie usunięte i **woluminów** strony zostaną wyświetlone zaktualizowaną listę woluminy w kontenerze.

## <a name="monitor-a-volume"></a>Monitor woluminu
Monitorowanie woluminów umożliwia zbieranie I/E-statystyki związane z woluminu. Monitorowanie jest domyślnie włączona dla pierwszych 32 woluminów utworzonych przez Ciebie. Monitorowanie z dodatkowych woluminów jest domyślnie wyłączone. Monitorowanie woluminów sklonowany jest również domyślnie wyłączone.

Wykonaj poniższe kroki, aby włączyć lub wyłączyć monitorowanie w woluminie.

### <a name="to-enable-or-disable-volume-monitoring"></a>Aby włączyć lub wyłączyć monitorowanie woluminu
1. Na **urządzeń** , wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij przycisk **kontenery woluminów** kartę.
2. Wybierz kontener woluminów, w której znajduje się wolumin, a następnie kliknij kontener woluminów, aby uzyskać dostęp do **woluminów** strony.
3. Wszystkie woluminy, które są skojarzone z tym kontenerem są wyświetlane w tabelaryczny widok. Kliknij i zaznacz woluminu lub Klonowanie woluminów.
4. W dolnej części strony kliknij **Modyfikuj**.
5. W Kreatorze modyfikowania woluminu w obszarze **podstawowe ustawienia**, wybierz pozycję **włączyć** lub **wyłączyć** z **monitorowanie** listy rozwijanej.
   
    ![Zmodyfikuj ustawienia podstawowe woluminu](./media/storsimple-manage-volumes/HCS_MonitorVolumeM.png)

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [sklonować woluminu StorSimple](storsimple-clone-volume.md).
* Dowiedz się, jak [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md).

