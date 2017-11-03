---
title: "Zarządzanie woluminy StorSimple (U2) | Dokumentacja firmy Microsoft"
description: "Wyjaśniono, jak dodawać, modyfikować, monitorowania i Usuń woluminy StorSimple i sposób wykonania ich w trybie offline, jeśli to konieczne."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 57896932-0aa5-4805-970c-d13403ae7551
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/28/2016
ms.author: alkohli
ms.openlocfilehash: a61c57cd74a0df8363648dd8df40e433b0e6489d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="use-the-storsimple-manager-service-to-manage-volumes-update-2"></a>Usługę Menedżer StorSimple umożliwia zarządzanie woluminami (Update 2)
[!INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Omówienie
W tym samouczku opisano sposób korzystania z usługi Menedżer StorSimple, tworzenie i zarządzanie nimi woluminów na urządzeniu StorSimple oraz urządzenia wirtualnego StorSimple z aktualizacji 2.

Usługę Menedżer StorSimple jest rozszerzeniem w klasycznym portalu Azure, która umożliwia zarządzanie rozwiązania StorSimple z interfejsem sieci web jednej. Oprócz zarządzania woluminami, można użyć usługi Menedżer StorSimple do tworzenia i zarządzania usługi StorSimple, wyświetlanie i zarządzanie urządzeniami, wyświetlanie alertów i wyświetlić i zarządzanie zasadami tworzenia kopii zapasowej i kopii zapasowej wykazu.

## <a name="volume-types"></a>Typy woluminu
Woluminy StorSimple można:

* **Przypięty lokalnie woluminach**: dane w tych woluminów pozostanie na lokalnym urządzeniu StorSimple przez cały czas.
* **Woluminami warstwowymi**: dane w tych woluminów mogą zostaną przeniesione do chmury.

Archiwizacja wolumin jest typem woluminu warstwowego. Większy rozmiar fragmentu deduplikacji używana w przypadku woluminów archiwizacji umożliwia urządzeniu transfer większej segmentów danych do chmury. 

Jeśli to konieczne, można zmienić woluminu typu z lokalnym warstwowej lub z warstwy do lokalnego. Aby uzyskać więcej informacji, przejdź do [zmienić typ woluminu](#change-the-volume-type).

### <a name="locally-pinned-volumes"></a>Woluminów przypiętych lokalnie
Woluminów przypiętych lokalnie to pełni woluminy, które wykonują nie warstwy danych w chmurze, zapewniając lokalnego gwarantuje dla danych podstawowych, niezależnie od łączności chmury. Dane dotyczące woluminów przypiętych lokalnie nie jest deduplikowany i skompresowane; jednak migawek woluminów przypiętych lokalnie są deduplikowane. 

Woluminów przypiętych lokalnie są w pełni zaaprowizowanym; w związku z tym musi mieć wystarczającą ilość miejsca na urządzeniu, podczas ich tworzenia. Można alokować woluminy przypięte lokalnie do 8 TB na urządzeniu StorSimple 8100 i 20 TB na urządzeniu 8600 maksymalnego rozmiaru. StorSimple rezerwuje pozostałe lokalne miejsce na urządzeniu do migawki, metadane i przetwarzania danych. Można zwiększyć rozmiar woluminu przypiętego lokalnie do maksymalne dostępne miejsce, ale nie można zmniejszyć rozmiar woluminu raz utworzony.

Podczas tworzenia woluminu przypiętego lokalnie, miejsce dostępne na potrzeby tworzenia woluminów warstwowych zostanie zmniejszona. To również sytuacja odwrotna: Jeśli masz istniejące woluminy warstwowe do tworzenia lokalnie przypięty woluminy miejsca będzie niższa niż maksymalnych podanych powyżej. Aby uzyskać więcej informacji na woluminach lokalnych, zapoznaj się [— często zadawane pytania na woluminów przypiętych lokalnie](storsimple-local-volume-faq.md).   

### <a name="tiered-volumes"></a>Woluminy warstwowe
Woluminy warstwowe są alokowane elastycznie woluminów, w których często używanych danych pozostaje na urządzeniu lokalnym, a rzadziej używanych danych jest automatycznie warstwy do chmury. Alokowanie elastyczne jest technologii wirtualizacji, dostępny magazyn prawdopodobnie przekracza zasobów fizycznych. Zamiast z wyprzedzeniem rezerwowania wystarczającej ilości miejsca, StorSimple używa alokowania elastycznego można przydzielić tylko za mało miejsca, aby spełniały bieżące wymagania. Elastyczne rodzaj magazynu w chmurze ułatwia takie podejście, ponieważ StorSimple można zwiększyć lub zmniejszyć do zmieniających się wymagań dotyczących magazynu w chmurze.

Jeśli używasz woluminu warstwowego na potrzeby danych archiwalnych, wybranie opcji **Użyj tego woluminu w przypadku rzadziej używanych danych archiwalnych** pola wyboru zmienia rozmiar fragmentu deduplikacji dla woluminu na 512 KB. Jeśli nie zaznaczysz tej opcji, odpowiedni wolumin warstwowy użyje rozmiaru fragmentu wynoszącego 64 KB. Większy rozmiar fragmentu deduplikacji umożliwia urządzeniu usprawnienie transferu dużej ilości danych archiwalnych w chmurze.

> [!NOTE]
> Archiwizacja woluminów utworzonych przy użyciu wersji 2 przed aktualizacją StorSimple będzie zaimportowany jako warstwowych archiwizacji jest zaznaczone pole wyboru.
> 
> 

### <a name="provisioned-capacity"></a>Udostępnione pojemności
Zapoznaj się z poniższej tabeli maksymalną pojemność udostępnione dla każdego typu urządzenia i woluminu. (Należy pamiętać, że woluminy przypięte lokalnie nie są dostępne na urządzeniu wirtualnym).

|  | Rozmiar maksymalny wolumin warstwowy | Rozmiar woluminu przypiętego lokalnie maksymalna |
| --- | --- | --- |
| **Fizyczne urządzenia** | | |
| 8100 |64 TB |8 TB |
| 8600 |64 TB |20 TB |
| **Urządzenia wirtualne** | | |
| 8010 |30 TB |Nie dotyczy |
| 8020 |64 TB |Nie dotyczy |

## <a name="the-volumes-page"></a>Strony woluminów
**Woluminów** strona umożliwia zarządzanie woluminami magazynu, które są udostępniane na urządzeniu Microsoft Azure StorSimple na potrzeby Twojego inicjatorów (serwerów). Wyświetli listę woluminów na urządzeniu StorSimple.

 ![Strony woluminów](./media/storsimple-manage-volumes-u2/VolumePage.png)

Wolumin składa się z szeregu atrybuty:

* **Nazwa woluminu** — opisową nazwę, która musi być unikatowa i pomaga w identyfikacji woluminu. Ta nazwa jest również używana do monitorowania raportów podczas filtrowania w określonym woluminie.
* **Stan** — może być w trybie online lub offline. Jeśli wolumin jest w trybie offline, nie jest widoczny dla inicjatorów (serwerów), które mają dostęp do korzystać z woluminu.
* **Pojemność** — określa łączną ilość danych, które mogą być przechowywane przez inicjatora (serwer). Przypięty lokalnie woluminy są w pełni zaaprowizowanym i znajdują się na urządzeniu StorSimple. Woluminy warstwowe są alokowane elastycznie i jest deduplikowany danych. Z woluminy alokowane elastycznie urządzenia nie wstępnie przydzielić pojemności magazynu fizycznego wewnętrznych lub w chmurze zgodnie ze skonfigurowanym pojemność. Pojemność woluminu jest przydzielone i używane na żądanie.
* **Typ** — wskazuje, czy wolumin jest **warstwowego** (ustawienie domyślne) lub **przypięty lokalnie**.
* **Kopia zapasowa** — wskazuje, czy domyślne zasady tworzenia kopii zapasowej istnieje dla tego woluminu.
* **Dostęp** — określa inicjatorów (serwerów), które mają prawo dostępu do tego woluminu. Inicjatory, względem których nie są elementami członkowskimi rekord kontroli dostępu (ACR), który jest skojarzony z woluminem nie będą widzieć woluminu.
* **Monitorowanie** — Określa, czy jest monitorowany wolumin. Wolumin ma monitorowania włączona domyślnie po jego utworzeniu. Monitorowanie będzie, jednak, można wyłączyć dla Klonowanie woluminów. Aby włączyć monitorowanie dla woluminu, postępuj zgodnie z instrukcjami [monitorować woluminu](#monitor-a-volume). 

Wykonaj instrukcje w tym samouczku, aby wykonywać następujące zadania:

* Dodawanie woluminu 
* Modyfikowanie woluminu 
* Zmień typ woluminu
* Usuwanie woluminu 
* Przełącz do trybu offline woluminu 
* Monitor woluminu 

## <a name="add-a-volume"></a>Dodawanie woluminu
Możesz [utworzony wolumin](storsimple-deployment-walkthrough-u2.md#step-6-create-a-volume) podczas wdrażania rozwiązania StorSimple. Dodawanie woluminu jest podobnej procedury.

#### <a name="to-add-a-volume"></a>Aby dodać wolumin
1. Na **urządzeń** , wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij przycisk **kontenery woluminów** kartę.
2. Wybierz kontener woluminów z listy i kliknij dwukrotnie, aby uzyskać dostęp do woluminów skojarzone z kontenera.
3. Kliknij przycisk **Dodaj** w dolnej części strony. Dodaj uruchamia Kreatora woluminów.
   
     ![Kreator dodawania woluminem ustawienia podstawowe](./media/storsimple-manage-volumes-u2/TieredVolEx.png)
4. W Kreatorze dodawania woluminu w obszarze **Ustawienia podstawowe** wykonaj następujące czynności:
   
   1. Wypełnij pole **Nazwa** dla woluminu.
   2. Wybierz **typ użycia** z listy rozwijanej. W przypadku obciążeń, które wymagają dane mają być dostępne lokalnie na urządzeniu przez cały czas, wybrać **przypięty lokalnie**. Dla wszystkich innych typów danych, wybierz **warstwowego**. (**Warstwowego** jest ustawieniem domyślnym.)
   3. W przypadku wybrania **warstwowego** w kroku 2, można wybrać **Użyj tego woluminu w przypadku rzadziej używanych danych archiwalnych** pole wyboru, aby skonfigurować archiwizacji woluminu.
   4. Wprowadź **alokowana pojemność** dla woluminu w GB lub TB. Zobacz [elastycznie pojemności](#provisioned-capacity) dla maksymalne rozmiary dla każdego typu urządzenia i woluminu. Przyjrzyj się **dostępnej pojemności** do ustalenia, ile miejsca do magazynowania jest faktycznie dostępne na urządzeniu.
5. Kliknij ikonę strzałki,![Ikona strzałki](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png). Jeśli konfigurujesz woluminu przypiętego lokalnie, pojawi się następujący komunikat.
   
    ![Komunikat typu woluminu zmiany](./media/storsimple-manage-volumes-u2/LocalVolEx.png)
6. Kliknij ikonę strzałki ![ikonę strzałki](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png)ponownie, aby przejść do **dodatkowe ustawienia** strony.
   
    ![Dodaj dodatkowe ustawienia kreatora woluminu](./media/storsimple-manage-volumes-u2/AddVolume2.png)<br>
7. W obszarze **dodatkowe ustawienia**, Dodaj nowy rekord kontroli dostępu (ACR):
   
   1. Wybierz rekord kontroli dostępu (ACR) z listy rozwijanej. Alternatywnie możesz dodać nowe ACR. ACRs ustalić, które hosty mogą uzyskiwać dostęp do woluminów dopasowując nazwy IQN hosta z wymienionego w rekordzie. Jeśli nie określisz ACR, pojawi się następujący komunikat.
      
        ![Określ ACR](./media/storsimple-manage-volumes-u2/SpecifyACR.png)
   2. Zaleca się, że wybrano **włączyć domyślnego tworzenia kopii zapasowej dla tego woluminu** wyboru.
   3. Kliknij ikonę znacznika wyboru ![Ikona znacznika wyboru](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) Aby utworzyć wolumin z określonymi ustawieniami.

Nowy wolumin jest teraz gotowa do użycia.

> [!NOTE]
> Jeśli tworzenia woluminu przypiętego lokalnie, a następnie utworzyć inny wolumin przypięty lokalnie natychmiast po tworzenia woluminu, które zadania są wykonywane sekwencyjnie. Pierwszy zadanie tworzenia woluminu musi zakończyć się przed rozpoczęciem następnego zadania tworzenia woluminu.
> 
> 

## <a name="modify-a-volume"></a>Modyfikowanie woluminu
Zmodyfikuj woluminu, gdy trzeba go rozwinąć lub zmienić hosty, które uzyskują dostęp do woluminu.

> [!IMPORTANT]
> * Jeśli zmodyfikujesz rozmiar woluminu na urządzeniu, rozmiar woluminu musi zostać zmienione na hoście, a także. 
> * Są opisane tutaj kroki po stronie hosta dla systemu Windows Server 2012 (2012 R2). Procedury dla systemu Linux lub z innymi systemami operacyjnymi hosta może się różnić. Zapoznaj się z instrukcjami podanymi przez system operacyjny hosta, modyfikując na woluminie z innym systemem operacyjnym hosta. 
> 
> 

#### <a name="to-modify-a-volume"></a>Aby zmodyfikować woluminu
1. Na **urządzeń** , wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij przycisk **kontenery woluminów** kartę.
2. Wybierz kontener woluminów z listy i kliknij dwukrotnie, aby wyświetlić skojarzone z kontenera woluminów.
3. Wybierz wolumin, a w dolnej części strony kliknij **Modyfikuj**. Zostanie uruchomiony Kreator woluminu Modyfikuj.
4. W Kreatorze modyfikowania woluminu w obszarze **podstawowe ustawienia**, można wykonaj następujące czynności:
   
   * Edytuj **nazwa**.
   * Konwertuj **typ użycia** z przypięte lokalnie do warstwy lub z warstwy do przypięty lokalnie (zobacz [zmienić typ woluminu](#change-the-volume-type) Aby uzyskać więcej informacji).
   * Zwiększ **elastycznie pojemności**. **Alokowana pojemność** zwiększyć. Nie można zmniejszyć wolumin, po jego utworzeniu.
5. W obszarze **dodatkowe ustawienia**, można zmodyfikować ACR, pod warunkiem, że wolumin jest w trybie offline. Jeśli wolumin jest w trybie online, należy przejść do trybu offline najpierw. Skorzystaj z procedury opisanej w [Przełącz do trybu offline wolumin](#take-a-volume-offline) przed modyfikowanie ACR.
   
   > [!NOTE]
   > Nie można zmienić **włączyć domyślnego tworzenia kopii zapasowej** opcji dla woluminu.
   > 
   > 
6. Zapisz zmiany, klikając ikonę znacznika wyboru ![ikona znacznika wyboru](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png). Klasyczny portal Azure wyświetli komunikat aktualizowania woluminu. Wyświetli komunikat informujący gdy wolumin został pomyślnie zaktualizowany.
7. Jeśli rozszerzasz woluminu, należy wykonać następujące czynności na komputerze hosta z systemem Windows:
   
   1. Przejdź do **Zarządzanie komputerem** ->**dysku zarządzania**.
   2. Kliknij prawym przyciskiem myszy **Zarządzanie dyskami** i wybierz **Skanuj dyski**.
   3. Na liście dysków, wybierz wolumin, który zostanie zaktualizowany, kliknij prawym przyciskiem myszy, a następnie wybierz **Rozszerz wolumin**. Zostanie uruchomiony Kreator rozszerzania woluminów. Kliknij przycisk **Dalej**.
   4. Ukończ pracę kreatora, akceptując wartości domyślne. Po zakończeniu pracy Kreatora woluminu powinien być wyświetlony zwiększenie wielkości.
      
      > [!NOTE]
      > Jeśli Rozszerz wolumin przypięty lokalnie, a następnie rozwiń węzeł przypięty lokalnie inny wolumin natychmiast po woluminu rozszerzenia zadania są wykonywane sekwencyjnie. Pierwszy zadania rozszerzenia woluminu musi zakończyć się przed rozpoczęciem następnego zadania rozszerzania woluminów.
      > 
      > 

![Zobacz film](./media/storsimple-manage-volumes-u2/Video_icon.png) **Zobacz film**

Aby obejrzeć film wideo przedstawiający sposób Rozszerz wolumin, kliknij przycisk [tutaj](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="change-the-volume-type"></a>Zmień typ woluminu
Można zmienić typ woluminu z warstwy do przypięty lokalnie lub z przypięty lokalnie do warstwowego. Jednak ta konwersja nie powinien być częste wystąpienie. Niektóre przyczyny Konwertowanie woluminu z warstwy do przypięty lokalnie są:

* Lokalnych gwarancji dotyczących wydajności i dostępności danych
* Eliminacja chmury opóźnienia i problemy z łącznością chmury.

Zazwyczaj są to mały istniejące woluminy, które mają być często uzyskuje dostęp do. Wolumin przypięty lokalnie jest w pełni zaaprowizowanym podczas jego tworzenia. Jeśli wolumin warstwowy są konwertowane na wolumin przypięty lokalnie, StorSimple sprawdza, czy masz wystarczającą ilość miejsca na urządzeniu przed rozpoczęciem konwersji. Jeśli masz za mało miejsca, zostanie wyświetlony błąd, a operacja zostanie anulowana. 

> [!NOTE]
> Przed rozpoczęciem konwersji do warstwy do przypięty lokalnie, upewnij się, należy rozważyć wymagania dotyczące miejsca na innych obciążeń. 
> 
> 

Możesz zmienić woluminu przypiętego lokalnie na wolumin warstwowy, jeśli potrzebujesz dodatkowego miejsca do obsługi administracyjnej inne woluminy. Podczas konwertowania woluminu przypiętego lokalnie do warstwy dostępnej pojemności na zwiększa urządzenia według rozmiaru wydanych pojemności. Jeśli problemy z łącznością zapobiec konwersji woluminu z lokalnego typu do typu warstwowych, lokalnym woluminie będzie mieć właściwości woluminu warstwowego do czasu ukończenia konwersji. Jest to spowodowane część danych może mieć rozrzucone do chmury. Te dane rozlanej będzie zajmować lokalne miejsce na urządzeniu, który nie jest możliwe, dopóki operacja jest uruchomiona ponownie oraz zostać zakończona.

> [!NOTE]
> Konwertowanie woluminu może zająć trochę czasu i nie można anulować konwersji po jego uruchomieniu. Gdy wolumin pozostaje w trybie online podczas konwersji i korzystać z kopii zapasowych, ale nie można rozwinąć lub przywracania woluminu, podczas konwersji.  
> 
> 

Konwersja z warstwowych do woluminu przypiętego lokalnie może niekorzystnie wpłynąć na wydajność urządzenia. Ponadto następujące czynniki może wydłużyć czas wymagany do ukończenia konwersji:

* Jest za mała przepustowość.
* Nie istnieje żadne obecnie wykonać kopii zapasowej.

Aby zminimalizować wpływ, które mogą być następujące czynniki:

* Zapoznaj się z przepustowości zasad i upewnij się, że dedykowany 40 MB/s przepustowości jest dostępny.
* Planowanie konwersji poza godzinami szczytu.
* Przed rozpoczęciem konwersji, należy utworzyć migawkę chmury.

Są konwersji wielu woluminów (obsługi różnych obciążeń), następnie należy powinien priorytet konwersji woluminu, aby wyższy priorytet woluminy są konwertowane najpierw. Na przykład należy przekonwertować woluminy, które obsługi maszyn wirtualnych (VM) lub z obciążeniami SQL przed dokonaniem konwersji woluminy z obciążeniami udziału plików.

#### <a name="to-change-the-volume-type"></a>Aby zmienić typ woluminu
1. Na **urządzeń** , wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij przycisk **kontenery woluminów** kartę.
2. Wybierz kontener woluminów z listy i kliknij dwukrotnie, aby wyświetlić skojarzone z kontenera woluminów.
3. Wybierz wolumin, a w dolnej części strony kliknij **Modyfikuj**. Zostanie uruchomiony Kreator woluminu Modyfikuj.
4. Na **podstawowe ustawienia** Zmień typ użycia przez wybranie nowego typu z **typ użycia** listy rozwijanej.
   
   * W przypadku zmiany typu **przypięty lokalnie**, StorSimple będzie sprawdzać, czy ma wystarczającej wydajności.
   * W przypadku zmiany typu **warstwowego** i ten wolumin, które będą używane na potrzeby danych archiwalnych, wybierz opcję **Użyj tego woluminu w przypadku rzadziej używanych danych archiwalnych** pole wyboru.
     
       ![Pole wyboru archiwum](./media/storsimple-manage-volumes-u2/ModifyTieredVolEx.png)
5. Kliknij ikonę strzałki ![ikonę strzałki](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) można przejść do **dodatkowe ustawienia** strony. Jeśli konfigurujesz woluminu przypiętego lokalnie, zostanie wyświetlony następujący komunikat.
   
    ![Komunikat typu woluminu zmiany](./media/storsimple-manage-volumes-u2/ModifyLocalVolEx.png)
6. Kliknij ikonę strzałki, ![ikona strzałki](./media/storsimple-manage-volumes-u2/HCS_ArrowIcon.png) ponownie, aby kontynuować.
7. Kliknij ikonę znacznika wyboru ![Ikona znacznika wyboru](./media/storsimple-manage-volumes-u2/HCS_CheckIcon.png) Aby rozpocząć proces konwersji. Azure portal wyświetli komunikat aktualizowania woluminu. Wyświetli komunikat informujący gdy wolumin został pomyślnie zaktualizowany.

## <a name="take-a-volume-offline"></a>Przełącz do trybu offline woluminu
Konieczne może być Przełącz wolumin do trybu offline podczas planowania go zmodyfikować lub usunąć. Gdy wolumin jest w trybie offline, nie jest dostępny do odczytu i zapisu. Należy podjąć woluminu w trybie offline na hoście, a także na urządzeniu. 

#### <a name="to-take-a-volume-offline"></a>Wolumin przełączenia do trybu offline
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

#### <a name="to-delete-a-volume"></a>Aby usunąć wolumin
1. Na **urządzeń** , wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij przycisk **kontenery woluminów** kartę.
2. Wybierz kontener wolumin zawierający wolumin, który chcesz usunąć. Kliknij przycisk kontenera woluminów, aby uzyskać dostęp do **woluminów** strony.
3. Wszystkie woluminy, które są skojarzone z tym kontenerem są wyświetlane w formacie tabelarycznym. Sprawdź stan woluminu, który chcesz usunąć. Jeśli wolumin do usunięcia nie jest w trybie offline, przejść do trybu offline należy wykonać kroki opisane w [Przełącz do trybu offline wolumin](#take-a-volume-offline).
4. Gdy wolumin jest w trybie offline, kliknij przycisk **usunąć** w dolnej części strony.
5. Po wyświetleniu monitu o potwierdzenie kliknij przycisk **Tak**. Wolumin zostanie usunięte i **woluminów** strony zostaną wyświetlone zaktualizowaną listę woluminy w kontenerze.
   
   > [!NOTE]
   > Jeśli usuniesz woluminu przypiętego lokalnie, miejsce dostępne dla nowych woluminów mogły nie zostać zaktualizowane od razu. Usługę Menedżer StorSimple okresowo aktualizuje lokalne miejsce dostępne. Sugerujemy, zaczekaj kilka minut, zanim zostanie podjęta próba utworzenia nowego woluminu.<br> Ponadto jeśli Usuń wolumin przypięty lokalnie, a następnie usuń innego lokalnie przypięty woluminu natychmiast po usuwanie woluminów, które zadania są wykonywane sekwencyjnie. Pierwszy zadania usuwania woluminu musi zakończyć się przed rozpoczęciem następnego zadania usuwania woluminu.
   > 
   > 

## <a name="monitor-a-volume"></a>Monitor woluminu
Monitorowanie woluminów umożliwia zbieranie I/E-statystyki związane z woluminu. Monitorowanie jest domyślnie włączona dla pierwszych 32 woluminów utworzonych przez Ciebie. Monitorowanie z dodatkowych woluminów jest domyślnie wyłączone. Monitorowanie woluminów sklonowany jest również domyślnie wyłączone.

Wykonaj poniższe kroki, aby włączyć lub wyłączyć monitorowanie w woluminie.

#### <a name="to-enable-or-disable-volume-monitoring"></a>Aby włączyć lub wyłączyć monitorowanie woluminu
1. Na **urządzeń** , wybierz urządzenie, kliknij go dwukrotnie, a następnie kliknij przycisk **kontenery woluminów** kartę.
2. Wybierz kontener woluminów, w której znajduje się wolumin, a następnie kliknij kontener woluminów, aby uzyskać dostęp do **woluminów** strony.
3. Wszystkie woluminy, które są skojarzone z tym kontenerem są wyświetlane w tabelaryczny widok. Kliknij i zaznacz woluminu lub Klonowanie woluminów.
4. W dolnej części strony kliknij **Modyfikuj**.
5. W Kreatorze modyfikowania woluminu w obszarze **podstawowe ustawienia**, wybierz pozycję **włączyć** lub **wyłączyć** z **monitorowanie** listy rozwijanej.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [sklonować woluminu StorSimple](storsimple-clone-volume.md).
* Dowiedz się, jak [zarządzać urządzenia StorSimple przy użyciu usługi Menedżer StorSimple](storsimple-manager-service-administration.md).

