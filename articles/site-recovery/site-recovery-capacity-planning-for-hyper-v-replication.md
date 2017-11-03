---
title: "Planisty wydajności funkcji Hyper-V dla usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób uruchamiania funkcji Hyper-V planisty wydajności dla usługi Azure Site Recovery"
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
ms.date: 10/30/2017
ms.author: nisoneji
ms.openlocfilehash: db790f9dc56605b5b752e7ab797903e32b2fc675
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="hyper-v-capacity-planner-tool-for-site-recovery"></a>Planisty wydajności funkcji Hyper-V dla usługi Site Recovery

W ramach wdrożenia usługi Azure Site Recovery należy dowiedzieć się, Twoje replikacji i wymaganiach odnośnie do przepustowości. Planisty wydajności funkcji Hyper-V dla usługi Site Recovery pomaga w tym celu replikacji maszyny wirtualnej funkcji Hyper-V.

W tym artykule opisano sposób uruchamiania narzędzi planowania pojemności funkcji Hyper-V. Tego narzędzia należy używać wraz z informacjami w [Planowanie wydajności dla usługi Site Recovery](site-recovery-capacity-planner.md).

## <a name="before-you-start"></a>Przed rozpoczęciem
Uruchom narzędzie na węzeł serwera lub klastra funkcji Hyper-V w lokacji głównej. Aby uruchomić narzędzie Serwery hosta funkcji Hyper-V musi:

* System operacyjny: Windows Server 2012 lub 2012 R2
* Pamięć: 20 MB (minimum)
* Procesora: obciążenie 5 procent (minimum)
* Miejsce na dysku: 5 MB (minimum)

Przed uruchomieniem narzędzia, które trzeba przygotować lokacji głównej. Jeśli replikujesz między dwoma lokacjami lokalnymi i chcesz sprawdzić przepustowości, należy przygotować jako serwer repliki.

## <a name="step-1-prepare-the-primary-site"></a>Krok 1: Przygotowanie lokacji głównej

1. W lokacji głównej należy utworzyć listę wszystkich maszyn wirtualnych funkcji Hyper-V mają być replikowane i hosty funkcji Hyper-V/klastrów na których są przechowywane. Narzędzie można uruchomić dla wielu autonomicznych hostów lub dla jednego klastra, ale nie oba jednocześnie. Również musi być ono uruchamiane osobno dla każdego systemu operacyjnego, więc należy zebrać informacje na temat serwerów funkcji Hyper-V w następujący sposób:

   * Samodzielne serwery usługi systemu Windows Server 2012
   * Klastry z systemem Windows Server 2012
   * Samodzielne serwery usługi Windows Server 2012 R2
   * Klastry z systemem Windows Server 2012 R2
2. Włącz dostęp zdalny z usługą WMI na wszystkich hostach funkcji Hyper-V i klastrów. Uruchom następujące polecenie na każdym/klastra serwera, aby upewnić się, że reguły zapory i uprawnienia użytkownika zostały ustawione:

        netsh firewall set service RemoteAdmin enable
3. Włącz wydajności na monitorowanie serwerów i klastrów, w następujący sposób:

   * Otwórz Zaporę systemu Windows z **zabezpieczeniami zaawansowanymi** przystawki, a następnie Włącz przychodzące następujące reguły: **dostęp sieciowy COM + (DCOM-IN)** i wszystkie reguły w **zdalnego dziennika zdarzeń Grupa zarządzania**.

## <a name="step-2-prepare-a-replica-server-on-premises-to-on-premises-replication"></a>Krok 2: Przygotowanie serwera repliki (lokalnej do lokalnej replikacji)
Nie należy to zrobić, jeśli przeprowadzasz replikację do platformy Azure.

Zaleca się jeden host funkcji Hyper-V jest skonfigurowany jako serwer odzyskiwania, dzięki czemu fikcyjny maszyny Wirtualnej mogą być replikowane na go, aby sprawdzić przepustowości.  Możesz to pominąć, ale nie będzie można pomiar przepustowości, chyba że użytkownik wykona.

1. Jeśli chcesz użyć węzła klastra jako replika skonfiguruj brokera funkcji Hyper-V Replica:

   * W **Menedżera serwera**, otwórz **Menedżera klastra trybu Failover**.
   * Połącz się z klastrem, zaznacz nazwę klastra i kliknij **akcje** > **Konfiguruj rolę** aby otworzyć Kreatora wysokiej dostępności.
   * W **wybierz rolę**, kliknij przycisk **brokera funkcji Hyper-V Replica**. W kreatorze należy podać **nazwę NetBIOS** i **adres IP** ma być używany jako punkt połączenia z klastrem (nazywany też punktem dostępu klienta). **Brokera funkcji Hyper-V Replica** zostanie skonfigurowana, co w nazwie punktu dostępu klienta, które należy zwrócić uwagę.
   * Sprawdź, czy rola brokera funkcji Hyper-V Replica pomyślnym przejściu do trybu online i może przełączyć się pomiędzy wszystkimi węzłami klastra. Aby to zrobić, kliknij prawym przyciskiem myszy rolę, wskaż pozycję **Przenieś**, a następnie kliknij przycisk **wybierz węzeł**. Wybierz węzeł > **OK**.
   * Jeśli używasz uwierzytelniania opartego na certyfikatach, upewnij się, każdy węzeł klastra i mieć zainstalowany certyfikat punkt dostępu klienta.
2. Włącz funkcję serwera repliki:

   * W klastrze Otwórz Menedżera klastra awarii, połącz się z klastrem i kliknij przycisk **ról** > Wybierz rolę > **ustawienia replikacji** > **Włącz ten klaster jako repliki Serwer**. Wybierz klaster jako replika, musisz mieć obecny rolę brokera funkcji Hyper-V Replica w klastrze w lokacji głównej.
   * Na autonomicznym serwerze otwórz Menedżera funkcji Hyper-V. W **akcje** okienku, kliknij przycisk **ustawienia funkcji Hyper-V** na serwerze, który chcesz włączyć, a w **konfiguracji replikacji** kliknij **Włącz ten komputer jako Serwer repliki**.
3. Konfigurowanie uwierzytelniania:

   * W **uwierzytelnianie i porty**, wybierz sposób uwierzytelniania podstawowego serwera i uwierzytelniania porty. Jeśli używasz kliknij certyfikat **wybierz certyfikat** można wybrać jedną. Użyj protokołu Kerberos, jeśli hosty funkcji Hyper-V podstawowymi i odzyskiwania znajdują się w tej samej domenie lub w domenach zaufanych. Wykorzystuje się certyfikaty dla różnych domenach lub wdrożenia grupy roboczej.
   * W **uwierzytelnianie i magazynowanie**, Zezwalaj na **żadnych** uwierzytelniony (podstawowemu) serwerowi na wysyłanie danych replikacji do tego serwera repliki.

     ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)
   * Uruchom **netsh http show servicestate**, aby sprawdzić, czy odbiornik jest uruchomiony dla określonego protokołu/port:  
4. Konfigurowanie zapór. Podczas instalacji funkcji Hyper-V reguły zapory są tworzone zezwalająca na ruch na domyślnym portów (HTTPS na 443, Kerberos na 80). Włącz te zasady w następujący sposób:
  - Certyfikat uwierzytelniania w klastrze (port 443):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}``
  - Uwierzytelnianie Kerberos w klastrze (80):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}``
  - Uwierzytelnianie certyfikatu na autonomicznym serwerze:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"``
  - Uwierzytelnianie Kerberos na autonomicznym serwerze:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"``

## <a name="step-3-run-the-capacity-planner-tool"></a>Krok 3: Uruchom narzędzie planowania pojemności
Po zostały przygotowane lokacji głównej i skonfigurować serwer odzyskiwania, należy uruchomić narzędzie.

1. [Pobierz](https://www.microsoft.com/download/details.aspx?id=39057) przez narzędzie Microsoft Download Center.
2. Uruchom narzędzie na jednym z serwerów podstawowych (lub jednego z węzłów z klastra głównego). Kliknij prawym przyciskiem myszy plik .exe, a następnie wybierz pozycję **Uruchom jako administrator**.
3. W **przed rozpoczęciem**, określ, jak długo mają być zbierane dane. Firma Microsoft zaleca się, że należy uruchomić narzędzie w celu zapewnienia danych reprezentatywnych poza godzinami produkcji. Jeśli próbujesz tylko Weryfikowanie łączności sieciowej, można zebrać tylko minutę.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)
4. W **szczegóły lokacji głównej**, określ nazwę serwera lub nazwa FQDN hosta autonomicznego lub klastra określić nazwę FQDN klienta zaakceptować punktu, klastra, nazwa lub dowolnego węzła w klastrze, a następnie kliknij przycisk **dalej**. Narzędzie automatycznie wykrywa nazwę serwera, na którym jest uruchomiona na. Narzędzie przejmuje maszyn wirtualnych, które mogą być monitorowane dla określonych serwerów.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)
5. W **szczegóły lokacji repliki**, jeśli przeprowadzasz replikację do platformy Azure, lub jeśli przeprowadzasz replikację do dodatkowego centrum danych i nie skonfigurowano połączenia z serwerem repliki, zaznacz **pominąć testy lokacji repliki**. Jeśli ustawiono typ repliki jest replikowana do pomocniczego centrum danych, wprowadź nazwę FQDN serwer autonomiczny lub punkt dostępu klienta dla klastra, w **Server name (lub) zakończenia brokera repliki funkcji Hyper-V**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)
6. W **rozszerzony replice szczegóły**, Włącz **Pomiń testy obejmujące lokacji rozszerzonej repliki**. Te testy nie są obsługiwane przez usługę Site Recovery.
7. W **wybierz maszyny wirtualne na ponowne zreplikowanie**, narzędzia łączy się serwer lub klaster i wyświetla maszyn wirtualnych i dyski uruchomionych na serwerze podstawowym, zgodnie z ustawieniami określone na **szczegóły lokacji głównej** Strona. Maszyny wirtualne, które są już włączone dla replikacji lub które nie są uruchomione, nie będą wyświetlane. Wybierz maszyny wirtualne, dla których chcesz zbierać metryki. Automatyczne zaznaczanie wirtualne dyski twarde zbiera dane dla maszyn wirtualnych za.
8. Jeśli skonfigurowano serwer repliki lub w klastrze, w **informacje o sieci**, określ przybliżonej przepustowości sieci WAN uważasz, że będą używane między lokacjami podstawowym i repliki, a następnie wybierz certyfikaty, jeśli skonfigurowano uwierzytelnianie certyfikatu.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)
9. W **Podsumowanie**, sprawdź ustawienia i kliknij przycisk **dalej** do rozpoczęcia zbierania metryk. Narzędzie postęp i stan jest wyświetlany na **obliczania pojemności** strony. Po zakończeniu działania narzędzia, kliknij przycisk **Wyświetl raport** Aby wyświetlić dane wyjściowe. Domyślnie dzienniki i raporty są przechowywane w **%systemdrive%\Users\Public\Documents\Capacity Planner**.

   ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)

## <a name="step-4-interpret-the-results"></a>Krok 4: Zrozumienie wyników

Poniżej przedstawiono ważne metryki. Metryki, które nie są wymienione w tym miejscu, można zignorować. Nie są one odpowiednie dla usługi Site Recovery.

### <a name="on-premises-to-on-premises-replication"></a>Dane lokalne na lokalnej replikacji

* Wpływ na replikację na podstawowy host obliczeniowych, pamięci
* Wpływ replikacji na serwerze podstawowym, miejsca na dysku magazynu hostów odzyskiwania, IOPS
* Całkowitej przepustowości wymagane dla replikacji różnicowej (MB/s)
* Przepustowość sieci obserwowanych między podstawowym hostem a hoście odzyskiwania (MB/s)
* Sugestia idealne liczby aktywnych równolegle transferu między dwa hosty/klastry

### <a name="on-premises-to-azure-replication"></a>Dane lokalne na replikację systemu Azure

* Wpływ na replikację na podstawowy host obliczeniowych, pamięci
* Wpływ replikacji hostem głównym magazynu miejsca na dysku, IOPS
* Całkowitej przepustowości wymagane dla replikacji różnicowej (MB/s)

## <a name="more-resources"></a>Więcej zasobów
* Aby uzyskać szczegółowe informacje o tym narzędziu odczytać dokument, który towarzyszy pobierania narzędzia.
* Obejrzyj wskazówki narzędzia na blogu Keitha Mayer [blogu w witrynie TechNet](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
* [Pobierz wyniki](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) z naszych testowanie wydajnościowe na potrzeby lokalnej do lokalnej replikacji funkcji Hyper-V

## <a name="next-steps"></a>Następne kroki

Po zakończeniu planowania pojemności możesz przystąpić do wdrażania usługi Site Recovery:

* [Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach VMM do platformy Azure](site-recovery-vmm-to-azure.md)
* [Replikowanie maszyn wirtualnych funkcji Hyper-V (bez VMM) na platformie Azure](site-recovery-hyper-v-site-to-azure.md)
* [Replikowanie maszyn wirtualnych funkcji Hyper-V między lokacjami programu VMM](site-recovery-vmm-to-vmm.md)
