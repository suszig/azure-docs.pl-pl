---
title: "Narzędzia do planowania dla usługi Azure Site Recovery pojemności funkcji Hyper-V | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób uruchamiania narzędzia planowania pojemności funkcji Hyper-V dla usługi Azure Site Recovery"
services: site-recovery
documentationcenter: na
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 2bc3832f-4d6e-458d-bf0c-f00567200ca0
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/28/2017
ms.author: nisoneji
ms.openlocfilehash: 80c3dcb65bd74bcfa3acc5f12dd5c45cd1c06455
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="hyper-v-capacity-planning-tool-for-site-recovery"></a>Narzędzia do planowania Site Recovery pojemności funkcji Hyper-V

Nowa wersja rozszerzone [Azure lokacji odzyskiwania wdrożenia Planistę dla funkcji Hyper-V do wdrożenia usługi Azure](site-recovery-hyper-v-deployment-planner.md) jest teraz dostępna. Zastępuje narzędzie stary. Użyj nowego narzędzia dla Planowanie wdrożenia. To narzędzie zawiera następujące wytyczne:

* Ocena kwalifikujących się maszyny Wirtualnej, na podstawie liczby dysków, rozmiar dysku IOPS, przenoszenie i kilka właściwości maszyny Wirtualnej
* Przepustowość sieci musi i oceny cel punktu odzyskiwania
* Wymagania dotyczące infrastruktury platformy Azure
* Wymagania dotyczące infrastruktury lokalnej
* Przetwarzanie wsadowe wskazówki dotyczące replikacji początkowej
* Szacowany koszt odzyskiwania po całkowitej awarii na platformie Azure

W ramach wdrożenia usługi Azure Site Recovery należy dowiedzieć się, Twoje replikacji i wymaganiach odnośnie do przepustowości. Pojemności funkcji Hyper-V, narzędzia do planowania Site Recovery pomaga określić następujące wymagania dotyczące replikacji maszyny Wirtualnej funkcji Hyper-V.

W tym artykule opisano sposób uruchamiania narzędzia do planowania pojemności funkcji Hyper-V. Za pomocą tego narzędzia, wraz z informacjami w [pojemności, planowanie odzyskiwania lokacji](site-recovery-capacity-planner.md).

## <a name="before-you-start"></a>Przed rozpoczęciem
Uruchom narzędzie na węzeł serwera lub klastra funkcji Hyper-V w lokacji głównej. Aby uruchomić narzędzie, serwery hosta funkcji Hyper-V należy:

* System operacyjny: Windows Server 2012 lub 2012 R2
* Pamięć: 20 MB (minimum)
* Procesora: obciążenie 5 procent (minimum)
* Miejsce na dysku: 5 MB (minimum)

Przed uruchomieniem narzędzia, które trzeba przygotować lokacji głównej. Jeśli replikujesz między dwiema lokacjami lokalnymi i chcesz sprawdzić przepustowości, które trzeba przygotować jako serwer repliki.

## <a name="step-1-prepare-the-primary-site"></a>Krok 1: Przygotowanie lokacji głównej

1. W lokacji głównej należy utworzyć listę wszystkich maszyn wirtualnych funkcji Hyper-V mają być replikowane. Wyświetl listę hostów funkcji Hyper-V lub klastry, gdzie są przechowywane. Narzędzie można uruchomić dla wielu autonomicznych hostów lub dla jednego klastra, ale nie oba jednocześnie. Również musi być ono uruchamiane osobno dla każdego systemu operacyjnego. Zbierz następujące informacje o serwerach funkcji Hyper-V:

   * Samodzielne serwery usługi systemu Windows Server 2012
   * Klastry z systemem Windows Server 2012
   * Samodzielne serwery usługi Windows Server 2012 R2
   * Klastry z systemem Windows Server 2012 R2

2. Włączenie dostępu zdalnego do Instrumentacji zarządzania Windows na wszystkich hostach funkcji Hyper-V i klastrów. Uruchom to polecenie dla każdego serwera lub klastra, aby upewnić się, że reguły zapory i ustawiania uprawnień użytkownika:

        netsh firewall set service RemoteAdmin enable
3. Włącz wydajności na monitorowanie serwerów i klastrów w następujący sposób:

   a. Otwórz Zaporę systemu Windows z **zabezpieczeniami zaawansowanymi** przystawki. 
   
   b. Wybierz regułę ruchu przychodzącego **dostęp sieciowy COM + (DCOM-IN)**. Wybierz wszystkie reguły w **zdalne zarządzanie dziennikiem zdarzeń** grupy.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>Krok 2: Przygotowanie serwera repliki (lokalnej do lokalnej replikacji)
Można replikować do platformy Azure, nie trzeba wykonać ten krok.

Zalecane jest skonfigurowanie jednego hosta funkcji Hyper-V jako serwer odzyskiwania, aby fikcyjny maszyny Wirtualnej mogą być replikowane na go, aby sprawdzić przepustowości. Ten krok można pominąć, ale nie pomiar przepustowości, chyba że użytkownik wykona.

1. Jeśli chcesz użyć węzła klastra jako replika, skonfiguruj brokera funkcji Hyper-V Replica:

   a. W **Menedżera serwera**, otwórz **Menedżera klastra trybu Failover**.

   b. Połącz się z klastrem, a następnie zaznacz nazwę klastra. Wybierz **akcje** > **Konfiguruj rolę** aby otworzyć Kreatora wysokiej dostępności.

   c. W **wybierz rolę**, wybierz pozycję **brokera funkcji Hyper-V Replica**. W kreatorze, wprowadź nazwę **nazwę NetBIOS**. Wprowadź adres dla **adres IP** ma być używany jako punkt połączenia z klastrem (nazywany też punktem dostępu klienta). **Brokera funkcji Hyper-V Replica** jest skonfigurowany, które powoduje nazwę punktu dostępu klienta, które należy zwrócić uwagę.

   d. Sprawdź, czy rola brokera funkcji Hyper-V Replica pomyślnym przejściu do trybu online i może przełączyć się pomiędzy wszystkimi węzłami klastra. Kliknij prawym przyciskiem myszy rolę, a następnie wybierz **Przenieś** > **wybierz węzeł**. Wybierz węzeł, a następnie wybierz **OK**.

   e. Jeśli korzystasz z uwierzytelniania opartego na certyfikatach, upewnij się, każdy węzeł klastra i mieć zainstalowany certyfikat punkt dostępu klienta.

2. Aby włączyć serwer repliki, należy wykonać następujące czynności:

   a. Dla klastra, otwórz **awarii klastra menedżera** i połącz się z klastrem. Wybierz **ról**, a następnie wybierz rolę. Wybierz **ustawienia replikacji** > **Włącz ten klaster jako serwer repliki**. Wybierz klaster jako replika, musisz mieć obecny rolę brokera funkcji Hyper-V Replica w klastrze w lokacji głównej.

   b. Serwer autonomiczny, otwórz **Menedżera funkcji Hyper-V**. W **akcje** okienku wybierz **ustawienia funkcji Hyper-V** na serwerze, który chcesz włączyć. W **konfiguracji replikacji**, wybierz pozycję **Włącz ten komputer jako serwer repliki**.

3. Aby skonfigurować uwierzytelnianie, wykonaj następujące kroki:

   a. W obszarze **uwierzytelnianie i porty**, wybierz sposób uwierzytelniania podstawowego serwera i uwierzytelniania porty. Jeśli używasz certyfikatu wybierz **wybierz certyfikat** można wybrać jedną. Użyj protokołu Kerberos, jeśli hosty funkcji Hyper-V podstawowymi i odzyskiwania znajdują się w tej samej domenie lub w domenach zaufanych. Wykorzystuje się certyfikaty dla różnych domenach lub wdrożenia grupy roboczej.

   b. W obszarze **uwierzytelnianie i magazynowanie**, Zezwalaj na **żadnych** uwierzytelniony (podstawowemu) serwerowi na wysyłanie danych replikacji do tego serwera repliki.

   ![Konfiguracja replikacji](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)

   c. Aby sprawdzić, czy odbiornik jest uruchomiony dla protokołu lub określony port, uruchom **netsh http show servicestate**. 

4. Konfigurowanie zapór. Podczas instalacji funkcji Hyper-V reguły zapory są tworzone zezwalająca na ruch na domyślne porty (HTTPS na 443) i protokołu Kerberos na 80. Włącz te zasady w następujący sposób:

    - Certyfikat uwierzytelniania w klastrze (port 443):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}``
    - Uwierzytelnianie Kerberos w klastrze (80):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}``
    - Uwierzytelnianie certyfikatu na autonomicznym serwerze:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"``
    - Uwierzytelnianie Kerberos na autonomicznym serwerze:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"``

## <a name="step-3-run-the-capacity-planning-tool"></a>Krok 3: Uruchamianie narzędzia do planowania pojemności
Po Przygotowanie lokacji głównej i skonfigurować serwer odzyskiwania, należy uruchomić narzędzie.

1. [Pobierz](https://www.microsoft.com/download/details.aspx?id=39057) przez narzędzie Microsoft Download Center.

2. Uruchom narzędzie z jednego z serwerów podstawowych lub jednego z węzłów z klastra głównego. Kliknij prawym przyciskiem myszy plik .exe, a następnie wybierz pozycję **Uruchom jako administrator**.

3. W **przed rozpoczęciem**, określ, jak długo mają być zbierane dane. Firma Microsoft zaleca, należy uruchomić narzędzie w celu zapewnienia danych reprezentatywnych poza godzinami produkcji. Jeśli chcesz zweryfikować tylko łączności sieciowej, można zebrać tylko minutę.

    ![Przed rozpoczęciem](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)

4. W **szczegóły lokacji głównej**, określ nazwę serwera lub nazwa FQDN hosta autonomicznego. Dla klastra Określ nazwę FQDN punktu dostępu klienta, nazwę klastra lub dowolnego węzła w klastrze. Wybierz opcję **Dalej**. Narzędzie automatycznie wykrywa nazwę serwera, na którym jest uruchomiona na. Narzędzie przejmuje maszyn wirtualnych, które mogą być monitorowane dla określonych serwerów.

    ![Szczegóły lokacji głównej](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)

5. W **szczegóły lokacji repliki**, jeśli replikować do platformy Azure lub jeśli replikować do dodatkowego centrum danych, a nie skonfigurowano połączenia z serwerem repliki, wybierz **pominąć testy lokacji repliki**. Jeśli replikować do dodatkowego centrum danych i Ustaw typ repliki, wprowadź nazwę FQDN serwer autonomiczny lub punkt dostępu klienta dla klastra w **Server name (lub) zakończenia brokera repliki funkcji Hyper-V**.

    ![Szczegóły lokacji repliki](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)

6. W **rozszerzony replice szczegóły**, wybierz pozycję **Pomiń testy obejmujące lokacji rozszerzonej repliki**. Te testy nie są obsługiwane przez usługę Site Recovery.

7. W **wybierz maszyny wirtualne na ponowne zreplikowanie**, narzędzie nawiązuje połączenie z serwerem lub klastra. Wyświetla maszyn wirtualnych i dyski uruchomionych na serwerze podstawowym, na podstawie ustawień określonych w **szczegóły lokacji głównej** strony. Maszyny wirtualne, które są już włączone dla replikacji lub które nie są uruchomione, nie będą wyświetlane. Wybierz maszyny wirtualne, dla których chcesz zbierać metryki. Automatyczne zaznaczanie wirtualne dyski twarde zbiera dane dla maszyn wirtualnych, zbyt.

8. Jeśli skonfigurowania serwera repliki lub w klastrze, w **informacje o sieci**, określ przybliżonej przepustowości sieci WAN do użycia między lokacjami podstawowym i repliki. Jeśli skonfigurowano uwierzytelnianie oparte na certyfikatach, wybierz certyfikaty.

    ![Informacje o sieci](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)

9. W **Podsumowanie**, sprawdź ustawienia. Wybierz **dalej** do rozpoczęcia zbierania metryk. Narzędzie postęp i stan są wyświetlane na **obliczania pojemności** strony. Po zakończeniu działania Narzędzia wybierz **Wyświetl raport** Aby wyświetlić dane wyjściowe. Domyślnie dzienniki i raporty są przechowywane w **%systemdrive%\Users\Public\Documents\Capacity Planner**.

   ![Obliczenia pojemności](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)

## <a name="step-4-interpret-the-results"></a>Krok 4: Zrozumienie wyników

Poniżej przedstawiono ważne metryki. Metryki, które nie są wymienione w tym miejscu, można zignorować. Nie są one odpowiednie dla usługi Site Recovery.

### <a name="on-premises-to-on-premises-replication"></a>Dane lokalne na lokalnej replikacji

* Wpływ na replikację na podstawowy host obliczeniową i pamięć
* Wpływ na replikację na podstawowy host hosta odzyskiwania miejsca na dysku i IOPS
* Całkowitej przepustowości wymagane dla replikacji różnicowej (MB/s)
* Przepustowość sieci obserwowanych między podstawowym hostem a hoście odzyskiwania (MB/s)
* Sugestię idealne liczby aktywnych równoległych transferów między dwoma hostów lub klastrów

### <a name="on-premises-to-azure-replication"></a>Dane lokalne na replikację systemu Azure

* Wpływ na replikację na podstawowy host obliczeniową i pamięć
* Wpływ na replikację na miejsce na dysku magazynu i IOPS hostem głównym
* Całkowitej przepustowości wymagane dla replikacji różnicowej (MB/s)

## <a name="more-resources"></a>Więcej zasobów

* Aby uzyskać więcej informacji o tym narzędziu odczytać dokument, który towarzyszy pobierania narzędzia.
* Obejrzyj wskazówki narzędzia na blogu Keitha Mayer [blogu w witrynie TechNet](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
* [Pobierz wyniki](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) z testowanie wydajnościowe na potrzeby lokalnej do lokalnej replikacji funkcji Hyper-V.

## <a name="next-steps"></a>Kolejne kroki

Po zakończeniu planowania pojemności, można wdrożyć usługi Site Recovery:
* [Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach VMM do platformy Azure](site-recovery-vmm-to-azure.md)
* [Replikowanie maszyn wirtualnych funkcji Hyper-V (bez VMM) na platformie Azure](site-recovery-hyper-v-site-to-azure.md)
* [Replikowanie maszyn wirtualnych funkcji Hyper-V między lokacjami programu VMM](site-recovery-vmm-to-vmm.md)
