---
title: "Konfigurowanie wielościeżkowego wejścia/wyjścia na hoście podłączonym do wirtualnego StorSimple | Dokumentacja firmy Microsoft"
description: "Zawiera opis sposobu konfigurowania wielościeżkowego wejścia/wyjścia (MPIO) dla macierzy wirtualnego StorSimple podłączone do hosta z systemem Windows Server 2012 R2."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 5b7a7f99-ee5b-4b7d-ab32-483a5a1fa504
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/01/2017
ms.author: alkohli
ms.openlocfilehash: c75c6ed40754aee964e2b68f4f569dc1422507f2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-multipath-io-on-windows-server-host-for-the-storsimple-virtual-array"></a>Konfigurowanie wielościeżkowego We/Wy na hoście systemu Windows Server dla tablicy wirtualnego StorSimple
## <a name="overview"></a>Omówienie
W tym artykule opisano sposób instalowania funkcji wielościeżkowego wejścia/wyjścia (MPIO) na hoście z systemem Windows Server, zastosowania określonych ustawień konfiguracyjnych dla woluminów StorSimple — tylko do, a następnie sprawdź wielościeżkowego wejścia/wyjścia dla woluminów StorSimple. W procedurze założono, że tablica wirtualnego StorSimple 1200 z dwoma interfejsami sieciowymi jest podłączony do hosta systemu Windows Server z dwoma interfejsami sieciowymi. Informacje zawarte w tym artykule dotyczą tylko wirtualny tablicy. Aby uzyskać informacje dotyczące urządzeń z serii StorSimple 8000, przejdź do tematu [konfigurowanie wielościeżkowego wejścia/wyjścia dla hosta StorSimple](storsimple-configure-mpio-windows-server.md). 

Funkcja wielościeżkowego wejścia/wyjścia w systemie Windows Server pomaga kompilacji wysokiej dostępności i odporności na uszkodzenia magazynu konfiguracji. Wielościeżkowe wejście/wyjście używa nadmiarowych składników ścieżki fizycznej — kart, kabli i przełączników — do tworzenia ścieżek logicznych między serwerem a urządzeniem magazynującym. W przypadku awarii składnika, Ścieżka logiczna się niepowodzeniem, przyczyną logika wielościeżkowa używa ścieżki alternatywnej do operacji We/Wy, dzięki czemu aplikacje mogą nadal uzyskiwać dostępu do danych. Ponadto w zależności od konfiguracji wielościeżkowego wejścia/wyjścia mogą także podnieść wydajność przez ponowne równoważenie obciążenia we wszystkich tych ścieżek. Aby uzyskać więcej informacji, zobacz [Omówienie wielościeżkowego wejścia/wyjścia](https://technet.microsoft.com/library/cc725907.aspx "wielościeżkowego wejścia/wyjścia Przegląd oraz funkcje").

W przypadku wysokiej dostępności rozwiązania StorSimple należy skonfigurować wielościeżkowego wejścia/wyjścia na hostach z systemem Windows Server podłączonym do macierzy wirtualnego StorSimple (model 1200). Następnie może tolerować Serwery hosta, łącza, sieci lub interfejsu. 

Należy wykonać następujące kroki, aby skonfigurować wielościeżkowe wejście/wyjście: 

* Wymagania wstępne dotyczące konfiguracji
* Krok 1: Instalacja wielościeżkowego We/Wy na hoście systemu Windows Server
* Krok 2: Konfigurowanie wielościeżkowego wejścia/wyjścia dla woluminów StorSimple
* Krok 3: Woluminy StorSimple instalacji na hoście

W poniższych sekcjach omówiono każdego z powyższych kroków.

## <a name="prerequisites"></a>Wymagania wstępne
Ta sekcja zawiera szczegóły dotyczące wymagania wstępne dotyczące konfiguracji dla hostów Windows Server i Tablica wirtualnego.

### <a name="on-windows-server-host"></a>Na hoście systemu Windows Server
* Upewnij się, że hoście z systemem Windows Server ma włączone 2 interfejsów sieciowych.

### <a name="on-storsimple-virtual-array"></a>W macierzy wirtualnego StorSimple
* Tablica wirtualnych powinna być skonfigurowana jako serwer iSCSI. Aby dowiedzieć się więcej, zobacz [Konfigurowanie wirtualnego tablicy jako serwer iSCSI](storsimple-virtual-array-deploy3-iscsi-setup.md). Jeden lub więcej interfejsów sieciowych powinna być włączona na tablicy.   
* Interfejsy sieciowe w sieci wirtualnej macierzy powinny być dostępne z hosta systemu Windows Server.
* Należy utworzyć jeden lub więcej woluminów na tablica wirtualne StorSimple. Aby dowiedzieć się więcej, zobacz [Dodaj wolumin](storsimple-virtual-array-deploy3-iscsi-setup.md#step-3-add-a-volume) na tablica wirtualne StorSimple. W tej procedurze w macierzy wirtualnego utworzono 3 (woluminy 1 lokalnie przypięty i 2 warstwowych jak pokazano poniżej).
  
    ![mpio0](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio0.png)

### <a name="hardware-configuration-for-storsimple-virtual-array"></a>Konfiguracja sprzętu dla tablicy wirtualnego StorSimple
Na poniższym rysunku przedstawiono konfigurację sprzętu wysokiej dostępności i równoważenia obciążenia wielu ścieżek dla hostów Windows Server i tablicy wirtualnego StorSimple używane w tej procedurze.

![Konfiguracja sprzętu wielościeżkowego wejścia/wyjścia](./media/storsimple-virtual-array-configure-mpio-windows-server/1200hardwareconfig.png)

Jak pokazano na powyższej ilustracji:

* Tablica wirtualnego StorSimple udostępniane w ramach funkcji Hyper-V jest urządzeniem active jednego węzła, skonfigurowany jako serwer iSCSI.
* Dwa interfejsy sieci wirtualnej są włączone w macierzy. W lokalnego interfejsu użytkownika sieci web macierzy wirtualnego, sprawdź, czy dwa interfejsy sieciowe są włączone, przechodząc do **ustawienia sieciowe** w sposób przedstawiony poniżej:
  
    ![Interfejsy sieciowe włączone 1200](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio9.png)
  
    Należy pamiętać, adresy IPv4 interfejsów sieciowych włączone (Ethernet, Ethernet 2 domyślnie) i Zapisz do późniejszego użytku na hoście.
* Dwa interfejsy sieciowe są włączone na hoście z systemem Windows Server. Jeśli podłączonych interfejsów dla hosta i tablicy znajdują się w tej samej podsieci, będą 4 ścieżki dostępne. To miało miejsce w tej procedurze. Jednak w przypadku każdego interfejsu w interfejsie tablicy i hosta w innej podsieci IP (i nie wzajemnie obsługują routing), to tylko 2 ścieżki będą dostępne.

## <a name="step-1-install-mpio-on-the-windows-server-host"></a>Krok 1: Instalacja wielościeżkowego We/Wy na hoście systemu Windows Server
Wielościeżkowego wejścia/wyjścia jest opcjonalna funkcja w systemie Windows Server i nie jest instalowany domyślnie. Powinno być instalowane jako funkcja za pomocą Menedżera serwera. Do zainstalowania tej funkcji na hoście z systemem Windows Server, wykonaj poniższą procedurę.

[!INCLUDE [storsimple-install-mpio-windows-server-host](../../includes/storsimple-install-mpio-windows-server-host.md)]

## <a name="step-2-configure-mpio-for-storsimple-volumes"></a>Krok 2: Konfigurowanie wielościeżkowego wejścia/wyjścia dla woluminów StorSimple
Wielościeżkowego wejścia/wyjścia, należy skonfigurować tak, aby zidentyfikować woluminów StorSimple. Aby skonfigurować, aby rozpoznać woluminów StorSimple, wykonaj następujące kroki.

[!INCLUDE [storsimple-configure-mpio-volumes](../../includes/storsimple-configure-mpio-volumes.md)]

## <a name="step-3-mount-storsimple-volumes-on-the-host"></a>Krok 3: Woluminy StorSimple instalacji na hoście
Po skonfigurowaniu wielościeżkowego wejścia/wyjścia w systemie Windows Server woluminy utworzone dla tablicy StorSimple może być instalowany i następnie skorzystać z funkcji MPIO nadmiarowości. Aby zainstalować wolumin, wykonaj następujące kroki.

#### <a name="to-mount-volumes-on-the-host"></a>Aby zainstalować woluminy na hoście
1. Otwórz **właściwości inicjatora iSCSI** okno na hoście systemu Windows Server. Przejdź do **Menedżera serwera > pulpit nawigacyjny > Narzędzia > inicjatora iSCSI**.
2. W **właściwości inicjatora iSCSI** okno dialogowe, kliknij przycisk **odnajdywania**, a następnie kliknij przycisk **odnajdowanie portalu obiektu docelowego**.
3. W **odnajdowanie portalu obiektu docelowego** okno dialogowe, wykonaj następujące czynności:
   
    1. Wprowadź adres IP interfejsu sieciowego włączony, pierwszy na tablica wirtualne StorSimple. Domyślnie to **Ethernet**. 
    2. Kliknij przycisk **OK** aby powrócić do **właściwości inicjatora iSCSI** okno dialogowe.
     
    > [!IMPORTANT]
    > Jeśli używasz sieci prywatnej dla połączenia iSCSI, wprowadź adres IP, portu danych, do którego jest podłączony do sieci prywatnej.
     
4. Powtórz kroki 2 i 3 dla drugiego interfejsu sieciowego (na przykład, Ethernet 2) w macierzy. 
5. Wybierz **celów** karcie **właściwości inicjatora iSCSI** okno dialogowe. Powierzchni każdego woluminu dla wirtualnego macierzy, powinien zostać wyświetlony jako miejsce docelowe w obszarze **wykryte obiekty docelowe**. W takim przypadku czy wykryte trzy elementy docelowe (odpowiadający trzy woluminy).
   
    ![mpio1](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio1.png)
6. Kliknij przycisk **Connect** ustanowienie sesji iSCSI z macierzy wirtualne StorSimple. A **nawiązywanie połączenia z docelowym** zostanie wyświetlone okno dialogowe. Wybierz **Włącz wielościeżkowe** pole wyboru. Kliknij przycisk **zaawansowane**.
   
    ![mpio2](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio2.png)
7. W **Zaawansowane ustawienia** okno dialogowe, wykonaj następujące czynności:                                        
   
    1. Na **karty lokalnej** listy rozwijanej wybierz **inicjatora iSCSI firmy Microsoft**.
    2. Na **IP inicjatora** listy rozwijanej wybierz adres IP hosta.
    3. Na **portalu obiektu docelowego** IP listy rozwijanej wybierz adres IP interfejsu tablicy.
    4. Kliknij przycisk **OK** aby powrócić do **właściwości inicjatora iSCSI** okno dialogowe.
     
        ![mpio3](./media/storsimple-ova-configure-mpio-windows-server/mpio3.png)

8. Kliknij pozycję **Właściwości**. 
   
    ![mpio4](./media/storsimple-ova-configure-mpio-windows-server/mpio4.png)

9. W **właściwości** okno dialogowe, kliknij przycisk **dodać sesji**.
   
   ![mpio5](./media/storsimple-ova-configure-mpio-windows-server/mpio5.png)
10. W **nawiązywanie połączenia z docelowym** okno dialogowe, wybierz opcję **Włącz wielościeżkowe** pole wyboru. Kliknij przycisk **zaawansowane**.
11. W **Zaawansowane ustawienia** okno dialogowe:                                        
    
    1. Na **karty lokalnej** listy rozwijanej, wybierz inicjatora iSCSI firmy Microsoft.

    2. Na **IP inicjatora** listy rozwijanej wybierz adres IP odpowiadający hosta. W takim przypadku nawiązujesz połączenie na macierzy dwa interfejsy sieciowe jeden interfejs sieci na hoście. W związku z tym ten interfejs jest taka sama jak ta podana dla pierwszej sesji.

    3. Na **docelowy adres IP portalu** listy rozwijanej, wybierz adres IP drugi interfejs danych, włączone na tablicy.

    4. Kliknij przycisk **OK** aby powrócić do okna dialogowego właściwości inicjatora iSCSI. Dodano drugiej sesji do obiektu docelowego.
      
       ![mpio11](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio11.png)
    
    5. Po dodaniu żądanego sesji (ścieżki) w **właściwości inicjatora iSCSI** okno dialogowe, wybierz obiekt docelowy, a kliknij przycisk **właściwości**. Na karcie sesje **właściwości** okno dialogowe, Uwaga cztery sesji identyfikatorów, które odpowiadają permutacji możliwych ścieżek. Aby anulować sesję, zaznacz pole wyboru obok identyfikator sesji, a następnie kliknij przycisk **rozłączenia**.

    6. Aby wyświetlić urządzenia przedstawione w sesji, wybierz **urządzeń** kartę. Aby skonfigurować zasady wielościeżkowego wejścia/wyjścia dla wybranego urządzenia, kliknij przycisk **wielościeżkowego wejścia/wyjścia**.

    7. **Szczegóły** zostanie wyświetlone okno dialogowe. Na **wielościeżkowego wejścia/wyjścia** karcie można wybrać odpowiednie **zasady równoważenia obciążenia** ustawienia. Można również wyświetlić **Active** lub **wstrzymania** typu ścieżki.
12. Powtórz kroki od 8-11, aby dodać dodatkowych sesji (ścieżki) do obiektu docelowego. Dwa interfejsy na hoście i dwa na tablicy wirtualnych możesz dodać łącznie cztery sesji dla każdego obiektu docelowego. 
    
    ![mpio14](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio14.png)
13. Należy powtórzyć te kroki dla każdego woluminu (powierzchni jako miejsce docelowe).
    
    ![mpio15](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio15.png)
14. Otwórz **Zarządzanie komputerem** przechodząc do **Menedżera serwera > pulpit nawigacyjny > Zarządzanie komputerem**. W okienku po lewej stronie kliknij **magazynu > Zarządzanie dyskami**. Woluminy utworzone dla tablicy wirtualnego StorSimple, których są widoczne dla tego hosta zostanie wyświetlona w obszarze **Zarządzanie dyskami** jako nowe dyski.
15. Zainicjuj dysk, a następnie utwórz nowy wolumin. W trakcie format wybierz rozmiar jednostki alokacji (AUS) 64 KB. Należy powtórzyć dla wszystkich dostępnych woluminów.
    
    ![Zarządzanie dyskami](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio20.png)
16. W obszarze **Zarządzanie dyskami**, kliknij prawym przyciskiem myszy **dysku** i wybierz **właściwości**.
17. W **właściwości urządzenia dysku wielościeżkowe** okno dialogowe, kliknij przycisk **wielościeżkowego wejścia/wyjścia** kartę.
    
    ![Właściwości dysku](./media/storsimple-virtual-array-configure-mpio-windows-server/mpio21.png)
18. W **Nazwa DSM** kliknij **szczegóły** i upewnij się, że parametry są ustawione na parametrów domyślnych. Parametry domyślne są:
    
    * Ścieżka upewnij się, okres = 30
    * Liczba ponownych prób = 3
    * PDO Usuń okres = 20
    * Interwał ponawiania prób = 1
    * Sprawdź ścieżkę włączone = niezaznaczone.
    
    > [!NOTE]
    > **Nie należy modyfikować domyślnych parametrów.**
   
## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [przy użyciu usługi Menedżer StorSimple urządzenia do administrowania tablica wirtualnego StorSimple](storsimple-virtual-array-manager-service-administration.md).

