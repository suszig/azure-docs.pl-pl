---
title: Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM do platformy Azure | Microsoft Docs
description: "Opisuje sposób wdrożenia usługi Site Recovery w celu organizowania replikacji, pracy w trybie failover i odzyskiwania maszyn wirtualnych funkcji Hyper-V w chmurach VMM do platformy Azure."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/23/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 75653b84d6ccbefe7d5230449bea81f498e10a98
ms.openlocfilehash: bdf9ce3d4ac359aa4150bc8912ce8b8302828343


---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-the-azure-portal"></a>Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM do platformy Azure przy użyciu witryny Azure Portal

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](site-recovery-vmm-to-azure.md)
> * [Klasyczny portal Azure](site-recovery-vmm-to-azure-classic.md)
> * [Program PowerShell — model usługi Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [PowerShell — model klasyczny](site-recovery-deploy-with-powershell.md)


Usługa Azure Site Recovery — Zapraszamy!

Usługa Site Recovery na platformie Azure ułatwia realizację strategii zachowania ciągłości działania i odzyskiwania danych po awarii (BCDR). Umożliwia ona organizowanie replikacji lokalnych serwerów fizycznych i maszyn wirtualnych do chmury (platforma Azure) lub pomocniczego centrum danych. W przypadku wystąpienia awarii w lokalizacji głównej następuje przełączenie w trybie failover do lokalizacji dodatkowej, dzięki czemu aplikacje i obciążenia są nadal dostępne. Powrót po awarii może mieć miejsce do lokalizacji głównej, gdy powróci ona do normalnego działania. Dowiedz się więcej w temacie [Co to jest usługa Azure Site Recovery?](site-recovery-overview.md)

W tym artykule opisano sposób replikowania lokalnych maszyn wirtualnych funkcji Hyper-V zarządzanych w chmurach programu System Center VMM do platformy Azure przy użyciu usługi Azure Site Recovery w witrynie Azure Portal.

Po przeczytaniu tego artykułu możesz zamieścić komentarze w dolnej części strony. Zadawaj pytania techniczne na [Forum Usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="quick-reference"></a>Krótki przewodnik
Jeśli chcesz przeprowadzić pełne wdrożenie, zdecydowanie zalecamy wykonanie wszystkich kroków opisanych w artykule. Ale jeśli masz ograniczony czas, skorzystaj z tego krótkiego podsumowania.

| **Obszar** | **Szczegóły** |
| --- | --- |
| **Scenariusz wdrożenia** |Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM do platformy Azure przy użyciu witryny Azure Portal |
| **Wymagania dotyczące środowiska lokalnego** |Co najmniej jeden serwer programu VMM uruchomiony w programie System Center 2012 R2 oraz co najmniej jedna chmura.<br/><br/> Chmury powinny zawierać co najmniej jedną grupę hostów programu VMM.<br/><br/> Co najmniej jeden serwer funkcji Hyper-V w chmurze co najmniej z systemem Windows Server 2012 R2 z rolą Hyper-V lub z systemem Microsoft Hyper-V Server 2012 R2 i najnowszymi aktualizacjami.<br/><br/> Serwery programu VMM i hosty funkcji Hyper-V muszą mieć dostęp do Internetu oraz możliwość uzyskiwania dostępu do określonych adresów URL bezpośrednio lub za pośrednictwem serwera proxy. [Wszystkie szczegóły](#on-premises-prerequisites). |
| **Ograniczenia środowiska lokalnego** |Serwer proxy oparty na protokole HTTPS nie jest obsługiwany |
| **Dostawca/agent** |Replikowanie maszyny wirtualnej wymagają dostawcy usługi Azure Site Recovery.<br/><br/> Hosty funkcji Hyper-V wymagają agenta usługi Recovery Services.<br/><br/> Te składniki są instalowanie podczas wdrażania. |
|  **Wymagania platformy Azure** |Konto platformy Azure<br/><br/> Magazyn usługi Recovery Services<br/><br/> Konto magazynu LRS lub GRS w regionie magazynu<br/><br/> Konto usługi Storage w warstwie Standardowa<br/><br/> Sieć wirtualna platformy Azure w regionie magazynu. [Wszystkie szczegóły](#azure-prerequisites). |
|  **Ograniczenia platformy Azure** |Jeśli używasz magazynu GRS, musisz mieć inne konto magazynu LRS na potrzeby rejestrowania.<br/><br/> Kont magazynu utworzonych w witrynie Azure Portal nie można przenosić między grupami zasobów w ramach tej samej lub różnych subskrypcji. <br/><br/> Magazyn w warstwie Premium nie jest obecnie obsługiwany.<br/><br/> Sieci platformy Azure używanych na potrzeby usługi Site Recovery nie można przenosić między grupami zasobów w ramach tej samej lub różnych subskrypcji.
|  **Replikacja maszyny wirtualnej** |[Maszyny wirtualne muszą spełniać wymagania wstępne platformy Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements)<br/><br/>
|  **Ograniczenia replikacji** |Nie można replikować maszyn wirtualnych z systemem Linux i statycznym adresem IP.<br/><br/> Z replikacji można wykluczyć określone dyski, ale nie dysk systemu operacyjnego.
| **Kroki wdrażania** |1) Przygotowanie platformy Azure (subskrypcja, magazyn, sieć) -> 2) Przygotowanie środowiska lokalnego (mapowanie programu VMM i sieci) -> 3) Utworzenie magazynu usługi Recovery Services -> 4) Skonfigurowanie hostów programu VMM i funkcji Hyper-V -> 5) Skonfigurowanie ustawień replikacji -> 6) Włączenie replikacji -> 7) Testowanie replikacji i pracy w trybie failover. |

## <a name="site-recovery-in-the-azure-portal"></a>Usługa Site Recovery w portalu Azure

Platforma Azure ma dwa różne [modele wdrażania](../resource-manager-deployment-model.md) związane z tworzeniem zasobów i pracą z nimi — model usługi Azure Resource Manager i model klasyczny. Platforma Azure ma również dwa portale: klasyczną witrynę Azure Portal i witrynę Azure Portal. W tym artykule opisano sposób wdrażania w witrynie Azure Portal.


W tym artykule opisano sposób wdrażania w witrynie Azure Portal, która zapewnia usprawnione środowisko wdrażania. Klasyczny portal może służyć do obsługi istniejących magazynów. Nowe magazyny nie mogą być tworzone za pomocą klasycznego portalu.


## <a name="site-recovery-in-your-business"></a>Usługa Site Recovery w Twojej firmie

Organizacje wymagają strategii BCDR, która określa, w jaki sposób aplikacje i dane pozostają uruchomione i dostępne podczas planowanych lub nieplanowanych przerw w pracy oraz są przywracane do normalnych warunków roboczych z możliwie dużą prędkością. Usługa Site Recovery ma następujące możliwości:

* Ochrona poza siedzibą w przypadku aplikacji biznesowych działających na maszynach wirtualnych funkcji Hyper-V.
* Pojedyncza lokalizacja umożliwiająca konfigurowanie i monitorowanie replikacji, trybu failover i odzyskiwania oraz zarządzanie nimi.
* Proste przechodzenie do pracy w trybie failover na platformie Azure i przywracanie po awarii (odzyskiwanie) z platformy Azure do serwerów hostów funkcji Hyper-V w lokacji lokalnej.
* Plany odzyskiwania obejmujące wiele maszyn wirtualnych, dzięki którym obciążenia aplikacji w tych samych warstwach przechodzą razem do trybu failover.

## <a name="scenario-architecture"></a>Architektura scenariusza
Oto składniki scenariusza:

* **Serwer programu VMM**: lokalny serwer programu VMM z co najmniej jedną chmurą.
* **Host lub klaster funkcji Hyper-V**: serwery hostów lub klastry funkcji Hyper-V zarządzane w chmurach programu VMM.
* **Dostawca usługi Azure Site Recovery i agent usługi Recovery Services**: podczas wdrożenia instaluje się dostawcę usługi Azure Site Recovery na serwerze programu VMM oraz agenta usługi Microsoft Azure Recovery Services na serwerach hostów funkcji Hyper-V. Dostawca na serwerze programu VMM komunikuje się z usługą Site Recovery za pośrednictwem protokołu HTTPS (port 443), aby replikować aranżację. Agent na serwerze hosta funkcji Hyper-V domyślnie replikuje dane do magazynu Azure za pośrednictwem protokołu HTTPS (port 443).
* **Azure**: potrzebujesz subskrypcji platformy Azure, konta usługi Azure Storage do przechowywania replikowanych danych oraz sieci wirtualnej Azure, aby maszyny wirtualne Azure mogły połączyć się z siecią po przejściu w tryb failover.

![Topologia E2A](./media/site-recovery-vmm-to-azure/architecture.png)

## <a name="azure-prerequisites"></a>Wymagania wstępne platformy Azure
Na platformie Azure potrzebujesz następujących elementów:

| **Wymagania wstępne** | **Szczegóły** |
| --- | --- |
| **Konto platformy Azure** |Potrzebujesz konta platformy [Microsoft Azure](http://azure.microsoft.com/). Możesz rozpocząć od [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/). [Dowiedz się więcej](https://azure.microsoft.com/pricing/details/site-recovery/) o cenach usługi Site Recovery. |
| **Magazyn platformy Azure** |Potrzebujesz standardowego konta usługi Azure Storage do przechowywania replikowanych danych. Możesz użyć konta magazynu LRS lub GRS. Zalecamy użycie konta GRS, dzięki czemu dane będą odporne w przypadku regionalnej awarii lub sytuacji, w której nie można odzyskać regionu podstawowego. [Dowiedz się więcej](../storage/storage-redundancy.md). Konto musi znajdować się w tym samym regionie co magazyn Usług odzyskiwania.<br/><br/>Magazyn w warstwie Premium nie jest obecnie obsługiwany.<br/><br/> W przypadku przejścia do trybu failover replikowane dane są przechowywane w usłudze Azure Storage i tworzone są maszyny wirtualne Azure. <br/><br/> [Przeczytaj o](../storage/storage-introduction.md) usłudze Azure Storage. |
| **Sieć platformy Azure** |Potrzebujesz sieci wirtualnej platformy Azure, z którą maszyny wirtualne Azure będą nawiązywać połączenie w przypadku przejścia w tryb failover. Sieć musi znajdować się w tym samym regionie co magazyn Usług odzyskiwania. |

## <a name="on-premises-prerequisites"></a>Lokalne wymagania wstępne
Oto, co należy zapewnić lokalnie

| **Wymagania wstępne** | **Szczegóły** |
| --- | --- |
| **VMM** |Co najmniej jeden serwer programu VMM uruchomiony w programie System Center 2012 R2. Każdy serwer programu VMM powinien mieć co najmniej jedną skonfigurowaną chmurę. Chmura powinna zawierać:<br/><br/> Co najmniej jedną grupę hostów programu VMM.<br/><br/> Co najmniej jeden serwer hosta lub klaster funkcji Hyper-V w każdej grupie hostów.<br/><br/>[Dowiedz się więcej](http://social.technet.microsoft.com/wiki/contents/articles/2729.how-to-create-a-cloud-in-vmm-2012.aspx) o konfigurowaniu chmur VMM. |
| **Funkcja Hyper-V** |Serwery hostów funkcji Hyper-V muszą być uruchomione w **systemie Windows Server 2012 R2** z rolą Hyper-V lub w **systemie Microsoft Hyper-V Server 2012 R2** i muszą mieć zainstalowane najnowsze aktualizacje.<br/><br/> Serwer funkcji Hyper-V powinien zawierać co najmniej jedną maszynę wirtualną.<br/><br/> Serwer hosta lub klaster funkcji Hyper-V, który zawiera maszyny wirtualne do replikacji, musi być zarządzany w chmurze programu VMM.<br/><br/>Serwery funkcji Hyper-V powinny być podłączone do Internetu, bezpośrednio lub za pośrednictwem serwera proxy.<br/><br/>Serwery funkcji Hyper-V powinny mieć zainstalowane poprawki wymienione w artykule [2961977](https://support.microsoft.com/kb/2961977).<br/><br/>Serwery hostów funkcji Hyper-V wymagają dostępu do Internetu w celu przeprowadzenia replikacji danych do platformy Azure. |
| **Dostawca i agent** |Podczas wdrażania usługi Azure Site Recovery instalujesz dostawcę usługi Azure Site Recovery na serwerze programu VMM oraz agenta usługi Recovery Services na hostach funkcji Hyper-V. Dostawca i agent muszą nawiązywać połączenie z platformą Azure za pośrednictwem Internetu, bezpośrednio lub przez serwer proxy. Serwer proxy oparty na protokole HTTPS nie jest obsługiwany. Serwer proxy na serwerze programu VMM i hostach funkcji Hyper-V powinien zezwalać na dostęp do: <br/><br/> ``*.accesscontrol.windows.net``<br/><br/> ``*.backup.windowsazure.com``<br/><br/> ``*.hypervrecoverymanager.windowsazure.com``<br/><br/> ``*.store.core.windows.net``<br/><br/> ``*.blob.core.windows.net``<br/><br/> ``https://www.msftncsi.com/ncsi.txt``<br/><br/> ``time.windows.com``<br/><br/> ``time.nist.gov``<br/><br/> Jeśli masz reguły zapory oparte na adresie IP na serwerze VMM, sprawdź, czy reguły zezwalają na komunikację z platformą Azure.<br/><br/> Zezwól na użycie [zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) oraz portu&443; protokołu HTTPS.<br/><br/> Zezwól na użycie zakresów adresów IP dla regionu platformy Azure Twojej subskrypcji oraz regionu Zachodnie stany USA.<br/><br/> |

## <a name="protected-machine-prerequisites"></a>Wymagania wstępne dotyczące chronionej maszyny
| **Wymagania wstępne** | **Szczegóły** |
| --- | --- |
| **Chronione maszyny wirtualne** |Zanim skorzystasz z trybu failover dla maszyny wirtualnej, upewnij się, że nazwa, która została przypisana do maszyny wirtualnej Azure, jest zgodna z [wymaganiami wstępnymi dotyczącymi platformy Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements). Nazwę można zmodyfikować po włączeniu replikacji dla maszyny wirtualnej. <br/><br/> Pojemności poszczególnych dysków na chronionych komputerach nie powinny przekraczać 1023 GB. Maszyna wirtualna może mieć maksymalnie 64 dyski (w związku z tym maksymalnie 64 TB pojemności).<br/><br/> Klastry udostępnionych dysków gościa nie są obsługiwane.<br/><br/> Rozruch typu Unified Extensible Firmware Interface (UEFI)/Extensible Firmware Interface (EFI) nie jest obsługiwany.<br/><br/> Jeśli źródło maszyny wirtualnej ma wiele kart sieciowych, zostanie przekonwertowane na jedną kartę sieciową w przypadku przejścia do trybu failover na platformie Azure.<br/><br/>Ochrona maszyn wirtualnych funkcji Hyper-V z systemem Linux i statycznym adresem IP nie jest obsługiwana. |

## <a name="prepare-for-deployment"></a>Przygotowanie do wdrożenia
Aby przygotować się do wdrożenia, należy wykonać następujące czynności:

1. [Skonfiguruj sieć platformy Azure](#set-up-an-azure-network), w której maszyny wirtualne Azure zostaną umieszczone po przejściu do trybu failover.
2. [Skonfiguruj konto usługi Azure Storage](#set-up-an-azure-storage-account) dla replikowanych danych.
3. [Przygotuj serwer programu VMM](#prepare-the-vmm-server) na wdrożenie usługi Site Recovery.
4. [Przygotuj się do mapowania sieci](#prepare-for-network-mapping). Skonfiguruj sieci, aby umożliwić skonfigurowanie mapowania sieci podczas wdrażania usługi Site Recovery.

### <a name="set-up-an-azure-network"></a>Konfiguracja sieci platformy Azure
Potrzebujesz sieci platformy Azure, z którą będą się łączyć maszyny wirtualne platformy Azure utworzone po przejściu do trybu failover.

* Sieć powinna znajdować się w tym samym regionie co magazyn usługi Recovery Services.
* W zależności od modelu zasobu, który chcesz wykorzystać dla maszyn wirtualnych platformy Azure w trybie failover, musisz skonfigurować sieć platformy Azure w [modelu usługi Resource Manager](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) lub [modelu klasycznym](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
* Zaleca się skonfigurowanie sieci przed rozpoczęciem dalszych działań. Jeśli tego nie zrobisz, konfigurację będzie trzeba przeprowadzić podczas wdrażania usługi Site Recovery.
Należy pamiętać, że sieci platformy Azure używanych na potrzeby usługi Site Recovery nie można [przenosić](../azure-resource-manager/resource-group-move-resources.md) w ramach tej samej lub różnych subskrypcji.

### <a name="set-up-an-azure-storage-account"></a>Konfigurowanie konta usługi Azure Storage
* Potrzebujesz standardowego konta usługi Azure Storage do przechowywania danych replikowanych do platformy Azure. Konto musi znajdować się w tym samym regionie co magazyn Usług odzyskiwania.
* W zależności od modelu zasobu, który chcesz wykorzystać dla maszyn wirtualnych Azure w trybie failover, należy skonfigurować konto w [modelu Resource Manager](../storage/storage-create-storage-account.md) lub [modelu klasycznym](../storage/storage-create-storage-account-classic-portal.md).
* Zalecamy skonfigurowanie konta przed rozpoczęciem dalszych działań. Jeśli tego nie zrobisz, konfigurację będzie trzeba przeprowadzić podczas wdrażania usługi Site Recovery.
- Należy pamiętać, że kont magazynu używanych przez usługę Site Recovery nie można [przenosić](../azure-resource-manager/resource-group-move-resources.md) w ramach tej samej lub różnych subskrypcji.

### <a name="prepare-the-vmm-server"></a>Przygotowanie serwera programu VMM
* Upewnij się, że serwer programu VMM jest zgodny z [wymaganiami wstępnymi](#on-premises-prerequisites).
* Podczas wdrażania usługi Site Recovery możesz określić, że wszystkie chmury na serwerze VMM powinny być dostępne w portalu Azure. Jeśli chcesz, aby tylko wybrane chmury były dostępne w portalu, możesz włączyć takie ustawienie dla chmury w konsoli administracyjnej programu VMM.

### <a name="prepare-for-network-mapping"></a>Przygotowanie do mapowania sieci
Podczas wdrażania usługi Site Recovery musisz skonfigurować mapowanie sieci. Mapowanie sieci działa między źródłowymi sieciami maszyny wirtualnej VMM i docelowymi sieciami platformy Azure, aby umożliwić korzystanie z następujących funkcji:

* Maszyny, które są przełączane w tryb failover w tej samej sieci, mogą się ze sobą łączyć, nawet jeśli przeszły do trybu failover na różne sposoby lub w ramach innych planów odzyskiwania.
* Jeśli brama sieci została skonfigurowana w docelowej sieci platformy Azure, maszyny wirtualne Azure mogą łączyć się z lokalnymi maszynami wirtualnymi.
* Aby skonfigurować mapowanie sieci, należy wykonać następujące czynności:

  * Upewnij się, że maszyny wirtualne na źródłowym serwerze hosta funkcji Hyper-V są podłączone do sieci maszyny wirtualnej VMM. Ta sieć powinna być połączona z siecią logiczną skojarzoną z chmurą.
  * Sieć platformy Azure zgodnie z [powyższym](#set-up-an-azure-network) opisem
* [Dowiedz się więcej](site-recovery-network-mapping.md) o tym, jak działa mapowanie sieci.

## <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu Usług odzyskiwania
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Kliknij kolejno pozycje **Nowy** > **Zarządzanie** > **Usługi odzyskiwania**. Możesz też kliknąć kolejno pozycje **Przeglądaj** > **Magazyny Usług odzyskiwania** > **Dodaj**.

    ![Nowy magazyn](./media/site-recovery-vmm-to-azure/new-vault3.png)
3. W polu **Nazwa** wprowadź przyjazną nazwę identyfikującą magazyn. Jeśli masz więcej niż jedną subskrypcję, wybierz jedną z nich.
4. [Utwórz grupę zasobów](../azure-resource-manager/resource-group-template-deploy-portal.md) lub wybierz istniejącą. Określ region platformy Azure. Maszyny będą replikowane do tego regionu. Aby sprawdzić obsługiwane regiony, zobacz sekcję dotyczącą dostępności geograficznej w temacie [Szczegóły cennika usługi Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/)
5. Jeśli chcesz szybko uzyskać dostęp do magazynu z pulpitu nawigacyjnego, kliknij pozycję **Przypnij do pulpitu nawigacyjnego** > **Utwórz magazyn**.

    ![Nowy magazyn](./media/site-recovery-vmm-to-azure/new-vault-settings.png)

Nowy magazyn będzie wyświetlany w sekcji **Pulpit nawigacyjny** > **Wszystkie zasoby** oraz w głównym bloku **magazynów usług Recovery Services**.

## <a name="get-started"></a>Rozpoczęcie pracy

Usługa Site Recovery zawiera środowisko dla rozpoczynających pracę (Wprowadzenie), które pomaga wdrożyć usługę tak szybko, jak to możliwe. Wprowadzenie sprawdza warunki wstępne i przeprowadza użytkownika przez kroki wdrożenia usługi Site Recovery w odpowiedniej kolejności.

Wybiera się typ maszyn do replikacji oraz miejsce, do którego mają zostać replikowane. Przeprowadza się konfigurację serwerów lokalnych, kont usługi Azure Storage oraz sieci. Tworzy się zasady replikacji i przeprowadza planowanie pojemności. Po przygotowaniu infrastruktury włącza się replikację maszyn wirtualnych. Można uruchomić tryb failover dla wybranych maszyn lub utworzyć plany odzyskiwania, aby przenosić wiele maszyn wirtualnych w tryb failover.

Rozpocznij proces Wprowadzenie, wybierając sposób wdrożenia usługi Site Recovery. Przepływ Wprowadzenie zmienia się w niewielkim zakresie w zależności od wymagań dotyczących replikacji.

## <a name="step-1-choose-your-protection-goals"></a>Krok 1. Wybranie celów ochrony
Wybierz, co chcesz replikować, i miejsce, do którego chcesz przeprowadzać replikację.

1. W bloku **magazynów usług Recovery Services** wybierz magazyn i kliknij przycisk **Ustawienia**.
2. W sekcji **Wprowadzenie** kliknij **Site Recovery** > **Krok 1. Przygotowanie infrastruktury** > **Cel ochrony**.

    ![Wybieranie celów](./media/site-recovery-vmm-to-azure/choose-goals.png)
3. W obszarze **Cel ochrony** wybierz pozycję **Do platformy Azure** i wybierz opcję **Tak, z funkcją Hyper-V**. Wybierz opcję **Tak**, aby potwierdzić, że używasz programu VMM do zarządzania hostami funkcji Hyper-V i lokacją odzyskiwania. Następnie kliknij przycisk **OK**.

    ![Wybieranie celów](./media/site-recovery-vmm-to-azure/choose-goals2.png)

## <a name="step-2-set-up-the-source-environment"></a>Krok 2. Konfigurowanie środowiska źródłowego
Zainstaluj dostawcę usługi Azure Site Recovery na serwerze programu VMM i zarejestruj serwer w magazynie. Zainstaluj agenta Usług odzyskiwania Azure na hostach funkcji Hyper-V.

1. Kliknij kolejno pozycje **Krok 2. Przygotowanie infrastruktury** > **Źródło**.

    ![Konfiguracja źródła](./media/site-recovery-vmm-to-azure/set-source1.png)
2. W obszarze **Przygotowywanie źródła** kliknij pozycję **+ VMM**, aby dodać serwer programu VMM.

    ![Konfiguracja źródła](./media/site-recovery-vmm-to-azure/set-source2.png)
3. W bloku **Dodawanie serwera** sprawdź, czy **Serwer programu System Center VMM** pojawia się w sekcji **Typ serwera** i czy serwer VMM spełnia [warunki wstępne i wymagania dotyczące adresu URL](#on-premises-prerequisites).
4. Pobierz plik instalacyjny dostawcy usługi Azure Site Recovery.
5. Pobierz klucz rejestracji. Będzie on potrzebny po uruchomieniu Instalatora. Klucz jest ważny przez pięć dni po jego wygenerowaniu.

    ![Konfiguracja źródła](./media/site-recovery-vmm-to-azure/set-source3.png)
6. Zainstaluj dostawcę usługi Azure Site Recovery na serwerze VMM.

### <a name="set-up-the-azure-site-recovery-provider"></a>Konfigurowanie dostawcy usługi Azure Site Recovery
1. Uruchom plik Instalatora dostawcy.
2. W usłudze **Microsoft Update** można włączyć aktualizacje, aby aktualizacje dostawcy były instalowane zgodnie z zasadami Microsoft Update.
3. W obszarze **Instalacja** zaakceptuj lub zmodyfikuj domyślną lokalizację instalacji dostawcy i kliknij przycisk **Zainstaluj**.

    ![Lokalizacja instalacji](./media/site-recovery-vmm-to-azure/provider2.png)
4. Po zakończeniu instalacji kliknij przycisk **Zarejestruj**, aby zarejestrować serwer programu VMM w magazynie.
5. Na stronie **Ustawienia magazynu** kliknij przycisk **Przeglądaj**, aby wybrać plik klucza magazynu. Określ subskrypcję usługi Azure Site Recovery oraz nazwę magazynu.

    ![Rejestracja serwera](./media/site-recovery-vmm-to-azure/provider10.PNG)
6. W obszarze **Połączenie internetowe** określ, jak dostawca uruchomiony na serwerze próbujemy VMM będzie się łączyć z usługą Site Recovery za pośrednictwem Internetu.

   * Jeśli chcesz, aby dostawca łączył się bezpośrednio, wybierz opcję **Połącz bezpośrednio z usługą Azure Site Recovery bez serwera proxy**.
   * Jeśli istniejący serwer proxy wymaga uwierzytelniania lub chcesz użyć niestandardowego serwera proxy, wybierz opcję **Połącz z usługą Azure Site Recovery przy użyciu serwera proxy**.
   * Jeśli używasz niestandardowego serwera proxy, określ adres, port i poświadczenia.
   * W przypadku użycia serwera proxy już zezwolono na użycie adresów URL opisanych w [wymaganiach wstępnych](#on-premises-prerequisites).
   * W przypadku użycia niestandardowego serwera proxy konto Uruchom jako programu VMM (DRAProxyAccount) zostanie automatycznie utworzone przy użyciu określonych poświadczeń serwera proxy. Skonfiguruj serwer proxy tak, aby to konto mogło być pomyślnie uwierzytelnione. Ustawienia konta Uruchom jako programu VMM można zmodyfikować w konsoli programu VMM. W obszarze **Ustawienia** rozwiń pozycję **Zabezpieczenia** > **Konta Uruchom jako**, a następnie zmodyfikuj hasło dla konta DRAProxyAccount. Aby to ustawienie zostało zastosowane, należy ponownie uruchomić usługę programu VMM.

     ![Internet](./media/site-recovery-vmm-to-azure/provider13.PNG)
7. Zaakceptuj lub zmodyfikuj lokalizację certyfikatu SSL, który jest automatycznie generowany w związku z szyfrowaniem danych. Ten certyfikat jest wykorzystywany, jeśli włączysz szyfrowanie danych dla chmury chronionej przez platformę Azure w portalu usługi Azure Site Recovery. Przechowuj ten certyfikat w bezpiecznym miejscu. Po uruchomieniu trybu failover na platformie Azure certyfikat będzie potrzebny do odszyfrowania danych, jeśli włączono szyfrowanie.
8. W polu **Nazwa serwera** wprowadź przyjazną nazwę identyfikującą serwer VMM w magazynie. W konfiguracji klastra określ nazwę roli klastra VMM.
9. Włącz **synchronizację metadanych chmury**, jeśli chcesz synchronizować metadane dla wszystkich chmur na serwerze VMM w magazynie. To działanie ma miejsce tylko raz na każdym serwerze. Jeśli nie chcesz synchronizować wszystkich chmur, możesz nie zaznaczać tego ustawienia i synchronizować poszczególne chmury indywidualnie we właściwościach chmury w konsoli programu VMM. Kliknij przycisk **Zarejestruj**, aby zakończyć proces.

    ![Rejestracja serwera](./media/site-recovery-vmm-to-azure/provider16.PNG)
10. Rozpoczyna się rejestracja. Po zakończeniu rejestracji serwer jest wyświetlany w bloku **Ustawienia** > **Serwery** w magazynie.

#### <a name="command-line-installation-for-the-azure-site-recovery-provider"></a>Instalacja dostawcy usługi Azure Site Recovery przy użyciu wiersza polecenia
Dostawca usługi Azure Site Recovery może zostać zainstalowany z poziomu wiersza polecenia. Tej metody można użyć do zainstalowania dostawcy w rdzeniu serwera dla systemu Windows Server 2012 R2.

1. Pobierz plik instalacyjny dostawcy i klucz rejestracji do folderu. Na przykład C:\ASR.
2. W wierszu polecenia o podwyższonym poziomie uprawnień uruchom następujące polecenia, aby wyodrębnić Instalator dostawcy:

            C:\Windows\System32> CD C:\ASR
            C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
3. Uruchom to polecenie, aby zainstalować składniki:

            C:\ASR> setupdr.exe /i
4. Następnie uruchom następujące polecenia, aby zarejestrować serwer w magazynie:

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

Gdzie:

* **/Credentials**: jest obowiązkowym parametrem określającym lokalizację pliku klucza rejestracji.  
* **/FriendlyName**: obowiązkowy parametr dla nazwy serwera hosta funkcji Hyper-V, która będzie wyświetlana w portalu usługi Azure Site Recovery.
* * **/EncryptionEnabled**: jest opcjonalnym parametrem w przypadku replikacji maszyn wirtualnych funkcji Hyper-V w chmurach VMM do platformy Azure. Określ, jak chcesz zaszyfrować maszyny wirtualne na platformie Azure (szyfrowanie danych w stanie spoczynku). Upewnij się, że nazwa pliku ma rozszerzenie **pfx**. Szyfrowanie jest domyślnie wyłączone.
* **/proxyAddress**: opcjonalny parametr określający adres serwera proxy.
* **/proxyport**: opcjonalny parametr określający port serwera proxy.
* **/proxyUsername**: opcjonalny parametr określający nazwę użytkownika serwera proxy (jeśli serwer proxy wymaga uwierzytelniania).
* **/proxyPassword**: jest opcjonalnym parametrem, który określa hasło do uwierzytelniania przy użyciu serwera proxy (jeśli serwer proxy wymaga uwierzytelniania).

### <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a>Instalacja agenta Usług odzyskiwania Azure na hostach funkcji Hyper-V
1. Po skonfigurowaniu dostawcy należy pobrać plik instalacyjny agenta usług Azure Recovery Services. Uruchom Instalator na każdym serwerze funkcji Hyper-V w chmurze VMM.

    ![Lokacje funkcji Hyper-V](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)
2. W obszarze **Sprawdzanie wymagań wstępnych** kliknij przycisk **Dalej**. Wszystkie brakujące wymagania wstępne zostaną zainstalowane automatycznie.

    ![Wymagania wstępne dotyczące agenta Usług odzyskiwania](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)
3. W obszarze **Ustawienia instalacji** zaakceptuj lub zmodyfikuj lokalizację instalacji i lokalizację pamięci podręcznej. Możesz skonfigurować pamięć podręczną na dysku, który ma co najmniej 5 GB dostępnego miejsca, ale zalecamy dysk pamięci podręcznej z co najmniej 600 GB wolnego miejsca. Następnie kliknij pozycję **Zainstaluj**.
4. Po zakończeniu instalacji kliknij przycisk **Zamknij**, aby zakończyć.

    ![Rejestracja agenta MARS](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

#### <a name="command-line-installation-for-azure-site-recovery-services-agent"></a>Instalacja agenta Usług odzyskiwania Azure Site Recovery przy użyciu wiersza polecenia
Agenta Usług odzyskiwania Microsoft Azure można zainstalować z poziomu wiersza polecenia przy użyciu następującego polecenia:

     marsagentinstaller.exe /q /nu

#### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a>Konfigurowanie internetowego dostępu serwera proxy do usługi Site Recovery na hostach funkcji Hyper-V
Agent Usług odzyskiwania uruchomiony na hostach funkcji Hyper-V wymaga internetowego dostępu do platformy Azure, aby umożliwić replikację maszyny wirtualnej. Jeśli uzyskujesz dostęp do Internetu za pośrednictwem serwera proxy, skonfiguruj go w następujący sposób:

1. Otwórz przystawkę MMC usługi Microsoft Azure Backup na hoście funkcji Hyper-V. Domyślnie skrót do usługi Microsoft Azure Backup jest dostępny na pulpicie lub w folderze C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. W przystawce kliknij pozycję **Zmień właściwości**.
3. Na karcie **Konfiguracja serwera proxy** podaj informacje dotyczące serwera proxy.

    ![Rejestracja agenta MARS](./media/site-recovery-vmm-to-azure/mars-proxy.png)
4. Upewnij się, że agent może osiągnąć adresy URL opisane w [wymaganiach wstępnych](#on-premises-prerequisites).

## <a name="step-3-set-up-the-target-environment"></a>Krok 3. Konfigurowanie środowiska docelowego
Określ konto magazynu Azure do wykorzystania podczas replikacji i sieć platformy Azure, z którą maszyny wirtualne Azure będą się łączyć po przejściu w tryb failover.

1. Kliknij pozycję **Przygotowywanie infrastruktury** > **Cel**, a następnie wybierz subskrypcję i grupę zasobów, w której chcesz utworzyć maszyny wirtualne w trybie failover. Wybierz model wdrażania, którego chcesz użyć na platformie Azure (model usługi Resource Manager lub model klasyczny) dla maszyn wirtualnych w trybie failover.

    ![Magazyn](./media/site-recovery-vmm-to-azure/enablerep3.png)

2. Usługa Site Recovery sprawdza, czy masz co najmniej jedno zgodne konto magazynu Azure i co najmniej jedną sieć platformy Azure.
    ![Storage](./media/site-recovery-vmm-to-azure/compatible-storage.png)

4. Jeśli nie utworzono konta magazynu, a chcesz je utworzyć przy użyciu usługi Resource Manager, kliknij pozycję **+ Konto magazynu**, aby zrobić to w tym miejscu.  W bloku **Utwórz konto magazynu** określ nazwę konta, jego typ, subskrypcję i lokalizację. Konto powinno znajdować się w tej samej lokalizacji co magazyn Usług odzyskiwania.

   ![Magazyn](./media/site-recovery-vmm-to-azure/gs-createstorage.png)

   Należy pamiętać, że:

   * Jeśli chcesz utworzyć konto magazynu przy użyciu modelu klasycznego, należy to zrobić w witrynie Azure Portal. [Dowiedz się więcej](../storage/storage-create-storage-account-classic-portal.md)
   * Jeśli używasz konta usługi Premium Storage dla replikowanych danych, skonfiguruj dodatkowe, standardowe konto magazynu do przechowywania dzienników replikacji, które przechwytują zachodzące zmiany w danych lokalnych.
5. Jeśli nie utworzono sieci platformy Azure, a chcesz ją utworzyć przy użyciu usługi Resource Manager, kliknij pozycję **+ Sieć**, aby zrobić to w tym miejscu. W bloku **Utwórz sieć wirtualną** określ nazwę sieciową, zakres adresów, szczegóły podsieci, subskrypcję i lokalizację. Sieć powinna znajdować się w tej samej lokalizacji co magazyn Usług odzyskiwania.

   ![Sieć](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

   Jeśli chcesz utworzyć sieć przy użyciu modelu klasycznego, zrób to w witrynie Azure Portal. [Dowiedz się więcej](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Konfiguracja mapowania sieci
* [Przeczytaj](#prepare-for-network-mapping) szybkie omówienie funkcji mapowania sieci. [Przeczytaj ten artykuł ](site-recovery-network-mapping.md), aby uzyskać szczegółowe informacje.
* Sprawdź, czy maszyny wirtualne na serwerze VMM zostały połączone z siecią maszyny wirtualnej i czy utworzono przynajmniej jedną sieć wirtualną platformy Azure. Wiele sieci maszyn wirtualnych można mapować do pojedynczej sieci platformy Azure.

Skonfiguruj mapowanie w następujący sposób:

1. W pozycji **Ustawienia** > **Infrastruktura usługi Site Recovery** > **Mapowania sieci** > **Mapowanie sieci** kliknij ikonę **+ Mapowanie sieci**.

    ![Mapowanie sieci](./media/site-recovery-vmm-to-azure/network-mapping1.png)
2. W obszarze **Dodawanie mapowania sieci** wybierz źródłowy serwer programu VMM i ustaw **Azure** jako element docelowy.
3. Sprawdź subskrypcję i model wdrożenia po przejściu do trybu failover.
4. W obszarze **Sieć źródłowa** wybierz lokalną sieć maszyny wirtualnej do mapowania z listy skojarzonej z serwerem VMM.
5. W obszarze **Sieć docelowa** wybierz sieć platformy Azure, w której znajdą się repliki maszyn wirtualnych Azure po ich utworzeniu. Następnie kliknij przycisk **OK**.

    ![Mapowanie sieci](./media/site-recovery-vmm-to-azure/network-mapping2.png)

Oto, co się dzieje po rozpoczęciu mapowania sieci:

* Istniejące maszyny wirtualne w źródłowej sieci maszyn wirtualnych są podłączane do docelowej sieci po rozpoczęciu mapowania. Nowe maszyny wirtualne, połączone ze źródłową siecią maszyn wirtualnych, są łączone z mapowaną siecią platformy Azure po replikacji.
* Jeśli modyfikujesz istniejące mapowanie sieci, repliki maszyn wirtualnych są łączone z wykorzystaniem nowych ustawień.
* Jeśli sieć docelowa ma wiele podsieci i jedna z tych podsieci ma taką samą nazwę, jak podsieć, w której znajduje się źródłowa maszyna wirtualna, replika maszyny wirtualnej jest łączona z tą docelową podsiecią po przejściu do trybu failover.
* Jeśli nie istnieje docelowa podsieć o takiej samej nazwie, maszyna wirtualna jest łączona z pierwszą podsiecią w sieci.

## <a name="step-4-set-up-replication-settings"></a>Krok 4. Konfigurowanie ustawień replikacji
1. Aby utworzyć nowe zasady replikacji, kliknij kolejno pozycje **Przygotowanie infrastruktury** > **Ustawienia replikacji** > **+ Utwórz i skojarz**.

    ![Sieć](./media/site-recovery-vmm-to-azure/gs-replication.png)
2. W obszarze **Utwórz i skojarz zasady** określ nazwę zasad.
3. W obszarze **Częstotliwość kopiowania** określ, jak często mają być replikowane dane przyrostowe po replikacji początkowej (co 30 sekund, 5 lub 15 minut).
4. W obszarze **Przechowywanie punktu odzyskiwania** określ w godzinach, jak długie będzie okno przechowywania dla każdego punktu odzyskiwania. Chronione maszyny można odzyskać do dowolnego punktu w tym oknie.
5. W obszarze **Częstotliwość migawek spójności aplikacji** określ, jak często (1–12 godzin) będą tworzone punkty odzyskiwania zawierające migawki spójne z aplikacjami. Funkcja Hyper-V wykorzystuje dwa typy migawek — standardową migawkę, która jest przyrostową migawką całej maszyny wirtualnej oraz migawkę spójności aplikacji, która wykonuje migawkę danych aplikacji wewnątrz maszyny wirtualnej w danym punkcie w czasie. Migawki spójne z aplikacjami używają usługi Volume Shadow Copy (VSS), aby zapewnić, że aplikacje są w spójnym stanie podczas wykonywania migawki. Należy pamiętać, że jeśli migawki spójne z aplikacjami zostaną włączone, będzie to miało wpływ na wydajność aplikacji uruchomionych na źródłowych maszynach wirtualnych. Upewnij się, że ustawiona wartość jest mniejsza od liczby skonfigurowanych dodatkowych punktów odzyskiwania.
6. W obszarze **Czas rozpoczęcia replikacji początkowej** określ, kiedy rozpoczyna się replikacja początkowa. Replikacja odbywa się z wykorzystaniem przepustowości Internetu, dlatego warto zaplanować ją poza najbardziej obciążonymi godzinami.
7. W obszarze **Szyfrowanie danych przechowywanych na platformie Azure** określ, czy należy szyfrować dane w stanie spoczynku w usłudze Azure Storage. Następnie kliknij przycisk **OK**.

    ![Zasady replikacji](./media/site-recovery-vmm-to-azure/gs-replication2.png)
8. Jeśli utworzysz nowe zasady, zostaną one automatycznie skojarzone z chmurą VMM. Kliknij przycisk **OK**. Możesz skojarzyć dodatkowe chmury VMM (oraz maszyny wirtualne w tych chmurach) z zasadami replikacji w obszarze **Ustawienia** > **Replikacja** > nazwa_zasad > **Skojarz chmurę VMM**.

    ![Zasady replikacji](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="step-5-capacity-planning"></a>Krok 5. Planowanie pojemności
Teraz, po skonfigurowaniu podstawowej infrastruktury, pomyśl o planowaniu pojemności i zorientuj się, czy potrzebujesz dodatkowych zasobów.

Usługa Site Recovery zawiera planistę wydajności, który pomaga w przydzielaniu odpowiednich zasobów do środowiska źródłowego, składników usługi Site Recovery, sieci i magazynu. Planistę można uruchomić w trybie szybkim, aby uzyskać szacunkowe wartości oparte na średnich liczbach maszyn wirtualnych, dysków i pojemności magazynu lub w trybie szczegółowym, w którym należy wprowadzić wartości na poziomie obciążenia. Przed rozpoczęciem:

* Zebrać informacje o środowisku replikacji, w tym o maszynach wirtualnych, liczbie dysków na maszynę wirtualną oraz pojemności dysków.
* Oszacować szybkość codziennych zmian (przenoszenia) dla replikowanych danych. Możesz użyć [planisty wydajności dla replik funkcji Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057), aby to zrobić.

1. Kliknij pozycję **Pobierz**, aby pobrać narzędzie, i uruchom je. [Przeczytaj artykuł](site-recovery-capacity-planner.md) dołączony do narzędzia.
2. Po zakończeniu wybierz opcję **Tak** w polu **Czy uruchamiano planistę wydajności**?

   ![Planowanie pojemności](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

### <a name="network-bandwidth-considerations"></a>Zagadnienia dotyczące przepustowości sieci
Możesz użyć planisty wydajności, aby obliczyć wymaganą przepustowość dla replikacji (replikacja początkowa i przyrostowa). Aby kontrolować poziom użycia przepustowości podczas replikacji, możesz skorzystać z kilku opcji:

* **Ograniczanie przepustowości**: ruch funkcji Hyper-V replikowany do lokacji dodatkowej przechodzi przez określony host funkcji Hyper-V. Możesz ograniczyć przepustowość na serwerze hosta.
* **Dostosowanie przepustowości**: możesz wywrzeć wpływ na przepustowość wykorzystywaną podczas replikacji przy użyciu kilku kluczy rejestru.

#### <a name="throttle-bandwidth"></a>Ograniczanie przepustowości
1. Otwórz przystawkę MMC usługi Microsoft Azure Backup na serwerze hosta funkcji Hyper-V. Domyślnie skrót do usługi Microsoft Azure Backup jest dostępny na pulpicie lub w folderze C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. W przystawce kliknij pozycję **Zmień właściwości**.
3. Na karcie **Ograniczanie przepływności** wybierz opcję **Włącz ograniczanie użycia przepustowości internetowej dla operacji kopii zapasowych** i ustaw limity dla godzin roboczych i godzin innych niż godziny pracy. Prawidłowy zakres: od 512 Kb/s do 102 Mb/s na sekundę.

    ![Ograniczanie przepustowości](./media/site-recovery-vmm-to-azure/throttle2.png)

Możesz też użyć polecenia cmdlet [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx), aby ustawić ograniczanie przepływności. Oto przykład:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

**Set-OBMachineSetting -NoThrottle** wskazuje, że ograniczanie przepływności nie jest wymagane.

#### <a name="influence-network-bandwidth"></a>Wywieranie wpływu na przepustowość sieci
Wartość rejestru **UploadThreadsPerVM** określa liczbę wątków, które są używane do transferu danych (replikacja początkowa lub przyrostowa) dysku. Wyższa wartość zwiększa przepustowość sieciową używaną podczas replikacji. Wartość rejestru **DownloadThreadsPerVM** określa liczbę wątków używanych do transferu danych podczas powrotu po awarii.

1. W rejestrze przejdź do pozycji **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.

   * Zmodyfikuj wartość **UploadThreadsPerVM** (lub utwórz klucz, jeśli nie istnieje), aby kontrolować wątki używane podczas replikacji dysku.
   * Zmodyfikuj wartość **DownloadThreadsPerVM** (lub utwórz klucz, jeśli nie istnieje), aby kontrolować wątki używane w ruchu związanym z powrotem po awarii z platformy Azure.
2. Wartość domyślna to 4. W sieci „o nadmiarowych zasobach” należy zmienić wartości domyślne tych kluczy rejestru. Wartość maksymalna to 32. Monitoruj ruch, aby zoptymalizować tę wartość.

## <a name="step-6-enable-replication"></a>Krok 6. Włączanie replikacji

Teraz włącz replikację w następujący sposób:

1. Kliknij kolejno pozycje **Krok 2. Replikowanie aplikacji** > **Źródło**. Po włączeniu replikacji po raz pierwszy kliknij pozycję **+ Replikuj** w magazynie, aby włączyć replikację dla dodatkowych maszyn.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication1.png)
2. W bloku **Źródło** wybierz serwer programu VMM i chmurę, w której znajdują się hosty funkcji Hyper-V. Następnie kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication-source.png)
3. W obszarze **Cel** wybierz subskrypcję, model wdrożenia po przejściu do trybu failover oraz konto magazynu używane do przechowywania replikowanych danych.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication-target.png)
4. Wybierz konto magazynu, którego chcesz użyć. Jeśli chcesz użyć konta magazynu innego niż posiadane, możesz [utworzyć konto](#set-up-an-azure-storage-account). Aby utworzyć konto magazynu przy użyciu modelu Resource Manager, kliknij pozycję **Utwórz nowe**. Jeśli chcesz utworzyć konto magazynu przy użyciu modelu klasycznego, należy to zrobić [w witrynie Azure Portal](../storage/storage-create-storage-account-classic-portal.md). Następnie kliknij przycisk **OK**.
5. Wybierz sieć platformy Azure i podsieć, z którą nawiążą połączenie maszyny wirtualne platformy Azure, gdy zostaną uruchomione po przejściu do trybu failover. Wybierz opcję **Konfiguruj teraz dla wybranych maszyn**, aby zastosować ustawienia sieci do wszystkich maszyn wybranych do ochrony. Wybierz opcję **Konfiguruj później**, aby wybrać sieć platformy Azure dla poszczególnych maszyn. Jeśli chcesz użyć sieci innej niż posiadana, możesz [utworzyć sieć](#set-up-an-azure-network). Aby utworzyć sieć przy użyciu modelu Resource Manager, kliknij pozycję **Utwórz nowe**. Jeśli chcesz utworzyć sieć przy użyciu modelu klasycznego, zrób to [w witrynie Azure Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Wybierz podsieć, jeśli jest to konieczne. Następnie kliknij przycisk **OK**.
6. W pozycji **Maszyny wirtualne** > **Wybierz maszyny wirtualne** kliknij i zaznacz każdą maszynę, którą chcesz replikować. Możesz wybrać tylko te maszyny, dla których można włączyć replikację. Następnie kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication5.png)
7. W pozycji **Właściwości** > **Konfiguruj właściwości** wybierz system operacyjny dla wybranych maszyn wirtualnych oraz dysk systemu operacyjnego. Domyślnie do replikacji są wybierane wszystkie dyski maszyny wirtualnej. Możesz wykluczyć dyski z replikacji, aby zmniejszyć wykorzystanie przepustowości w przypadku replikacji zbędnych danych na platformie Azure. Możesz na przykład zrezygnować z replikacji dysków z danymi tymczasowymi lub danych odświeżanych podczas każdego ponownego uruchomienia maszyny lub aplikacji (na przykład pagefile.sys lub tempdb programu Microsoft SQL Server). Dysk można wykluczyć z replikacji, cofając jego zaznaczenie. Sprawdź, czy nazwa maszyny wirtualnej platformy Azure (nazwa docelowa) jest zgodna z [wymaganiami maszyny wirtualnej platformy Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements) i w razie potrzeby ją zmodyfikuj. Następnie kliknij przycisk **OK**. Później możesz ustawić dodatkowe właściwości.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication6-with-exclude-disk.png)

    >[!NOTE]
    >
    > * Tylko dyski podstawowe można wyłączyć z replikacji. Nie można wykluczyć dysku systemu operacyjnego. Nie jest zalecane wykluczanie dysków dynamicznych. Usługa ASR nie może ustalić, który wirtualny dysk twardy jest podstawowym, a który dynamicznym dyskiem wewnątrz maszyny wirtualnej gościa.  Jeśli wszystkie zależne dyski woluminu dynamicznego nie zostaną wykluczone, chroniony dysk dynamiczny będzie uszkodzonym dyskiem maszyny wirtualnej w trybie failover, a dane na tym dysku mogą być niedostępne.
    > * Po włączeniu replikacji nie można dodawać ani usuwać dysków do replikacji. Jeśli chcesz dodać lub wykluczyć dysk, musisz wyłączyć ochronę maszyny wirtualnej, a następnie włączyć ją ponownie.
    > * Jeśli wykluczasz dysk wymagany do działania aplikacji, po przejściu do trybu failover na platformie Azure musisz utworzyć go na tej platformie, aby można było uruchomić replikowaną aplikację. Można również zintegrować usługę Azure Automation w planie odzyskiwania, aby utworzyć dysk podczas pracy maszyny w trybie failover.
    > * Dyski tworzone ręcznie na platformie Azure nie mają możliwości powrotu po awarii. Jeśli na przykład przeniesiesz trzy dyski do trybu failover i utworzysz dwa bezpośrednio na maszynie wirtualnej platformy Azure, tylko dla trzech dysków przeniesionych do trybu failover nastąpi powrót po awarii z platformy Azure do funkcji Hyper-V. Dysków tworzonych ręcznie nie można uwzględnić podczas powrotu po awarii ani replikacji odwrotnej z funkcji Hyper-V do platformy Azure.
    >
    >


8. W pozycji **Ustawienia replikacji** > **Konfigurowanie ustawień replikacji** wybierz zasady replikacji, które chcesz zastosować dla chronionych maszyn wirtualnych. Następnie kliknij przycisk **OK**. Możesz zmodyfikować zasady replikacji w pozycji **Ustawienia** > **Zasady replikacji** > nazwa_zasady > **Edytuj ustawienia**. Zastosowane zmiany są używane dla obecnie replikowanych i nowych maszyn.

   ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication7.png)

Możesz śledzić postępy zadania **Włącz ochronę** w pozycji **Ustawienia** > **Zadania** > **Zadania usługi Site Recovery**. Po uruchomieniu zadania **Sfinalizuj ochronę** maszyna jest gotowa do przejścia w tryb failover.

### <a name="view-and-manage-vm-properties"></a>Wyświetlanie właściwości maszyny wirtualnej i zarządzanie nimi
Zalecamy zweryfikowanie właściwości maszyny źródłowej. Pamiętaj, że nazwa maszyny wirtualnej Azure powinna być zgodna z [wymaganiami dotyczącymi maszyn wirtualnych Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements).

1. Kliknij kolejno pozycje **Ustawienia** > **Elementy chronione** > **Elementy replikowane** i wybierz maszynę, aby zobaczyć szczegółowe informacje.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/vm-essentials.png)
2. W obszarze **Właściwości** możesz wyświetlić informacje dotyczące replikacji i trybu failover dla danej maszyny wirtualnej.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/test-failover2.png)
3. W pozycji **Obliczenia i sieć** > **Właściwości obliczeń** możesz określić nazwę i docelowy rozmiar maszyny wirtualnej Azure. Zmodyfikuj nazwę, aby była zgodna z [wymaganiami platformy Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements), jeśli zachodzi taka potrzeba. Możesz również wyświetlić i zmodyfikować informacje dotyczące sieci docelowej, podsieci i adresu IP, które zostaną przypisane do maszyny wirtualnej platformy Azure.
Należy pamiętać, że:

   * Możesz ustawić docelowy adres IP. Jeśli nie podasz adresu, maszyna w trybie failover będzie używać protokołu DHCP. Jeśli ustawisz adres, który nie jest dostępny w trybie failover, przejście do trybu failover zakończy się niepowodzeniem. Ten sam docelowy adres IP może być użyty do testowania trybu failover, jeśli adres jest dostępny w testowej sieci trybu failover.
   * Liczba kart sieciowych zależy od rozmiaru określonego dla docelowej maszyny wirtualnej, zgodnie z poniższym:

     * Jeśli liczba kart sieciowych w maszynie źródłowej jest mniejsza lub równa liczbie kart sieciowych dozwolonych dla rozmiaru maszyny docelowej, maszyna docelowa będzie mieć taką samą liczbę kart sieciowych jak maszyna źródłowa.
     * Jeśli liczba kart sieciowych dla źródłowej maszyny wirtualnej przekracza liczbę dozwolonych kart sieciowych dla rozmiaru maszyny docelowej, zostanie użyta maksymalna liczba kart dla rozmiaru maszyny docelowej.
     * Przykładowo, jeśli maszyna źródłowa ma dwie karty sieciowe, a rozmiar maszyny docelowej obsługuje cztery karty, maszyna docelowa będzie mieć dwie karty sieciowe. Jeśli maszyna źródłowa ma dwie karty sieciowe, ale rozmiar maszyny docelowej obsługuje tylko jedną kartę, maszyna docelowa będzie mieć tylko jedną kartę sieciową.     
     * Jeśli maszyna wirtualna ma kilka kart sieciowych, wszystkie będą łączyć się z tą samą siecią.

     ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/test-failover4.png)
4. W pozycji **Dyski** możesz zobaczyć dyski systemu operacyjnego i danych na maszynie wirtualnej, która będzie replikowana.

### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Przygotowanie do połączenia z maszynami wirtualnymi Azure po przejściu do trybu failover
Jeśli chcesz nawiązać połączenie z maszynami wirtualnymi Azure przy użyciu protokołu RDP po przejściu do trybu failover, upewnij się, że wykonano następujące czynności:

**Na maszynie lokalnej przed przejściem do trybu failover**:

* W przypadku dostępu przez Internet włącz protokół RDP, upewnij się, że reguły TCP i UDP zostały dodane do połączeń **publicznych**, a następnie upewnij się, że w pozycji **Zapora systemu Windows** -> **Dozwolone aplikacje i funkcje** zezwolono na użycie protokołu RDP we wszystkich profilach.
* W przypadku dostępu za pośrednictwem połączenia między lokacjami włącz protokół RDP na maszynie i upewnij się, że w pozycji **Zapora systemu Windows** -> **Dozwolone aplikacje i funkcje** zezwolono na użycie protokołu RDP dla sieci typu **Domena** i **Prywatne**.
* Zainstaluj [agenta maszyny wirtualnej Azure](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) na maszynie lokalnej.
* Upewnij się, że zasady sieci SAN systemu operacyjnego są ustawione na OnlineAll. [Dowiedz się więcej](https://support.microsoft.com/kb/3031135)
* Wyłącz usługę IPSec przed uruchomieniem trybu failover.

**Na maszynie wirtualnej Azure po przejściu do trybu failover**:

* Dodaj publiczny punkt końcowy dla protokołu RDP (port 3389) i określ poświadczenia logowania.
* Upewnij się, że nie ma żadnych zasad domeny, które uniemożliwiają połączenie z maszyną wirtualną przy użyciu adresu publicznego.
* Spróbuj nawiązać połączenie. Jeśli nie możesz się połączyć, sprawdź, czy maszyna wirtualna została uruchomiona. Aby uzyskać więcej porad dotyczących rozwiązywania problemów, przeczytaj ten [artykuł](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

Aby uzyskać dostęp do maszyny wirtualnej Azure z systemem Linux po przejściu do trybu failover przy użyciu klienta Secure Shell (ssh), wykonaj następujące czynności:

**Na maszynie lokalnej przed przejściem do trybu failover**:

* Upewnij się, że usługa Secure Shell na maszynie wirtualnej Azure jest ustawiona na automatyczne uruchomienie przy rozruchu systemu.
* Sprawdź, czy reguły zapory zezwalają na połączenie SSH.

**Na maszynie wirtualnej Azure po przejściu do trybu failover**:

* Reguły grupy zabezpieczeń sieci na maszynie wirtualnej w trybie failover i w podsieci platformy Azure, do której maszyna jest podłączona, muszą zezwalać na połączenia przychodzące do portu SSH.
* Aby zezwolić na połączenia przychodzące do portu SSH (domyślnie port TCP 22), należy utworzyć publiczny punkt końcowy.
* Jeśli dostęp do maszyny wirtualnej uzyskuje się przez połączenie sieci VPN (VPN Express Route lub między lokacjami), można użyć klienta do nawiązania bezpośredniego połączenia z maszyną wirtualną przez port SSH.


## <a name="step-7-test-your-deployment"></a>Krok 7. Testowanie wdrożenia
Aby przetestować wdrożenie, możesz uruchomić test trybu failover dla jednej maszyny wirtualnej lub plan odzyskiwania, który zawiera co najmniej jedną maszynę wirtualną.

1. Aby przenieść jedną maszynę wirtualną do trybu failover, w pozycji **Ustawienia** > **Elementy replikowane** kliknij maszynę wirtualną > **+ Testowy tryb failover**.
1. Aby przenieść plan odzyskiwania do trybu failover, w pozycji **Ustawienia** > **Plany odzyskiwania** kliknij plan prawym przyciskiem myszy > **Testowy tryb failover**. Aby utworzyć plan odzyskiwania, [wykonaj te instrukcje](site-recovery-create-recovery-plans.md).
1. W pozycji **Testowy tryb failover** wybierz sieć platformy Azure, z którą maszyny wirtualne Azure zostaną połączone po przejściu do trybu failover.
1. Kliknij przycisk **OK**, aby rozpocząć tryb failover. Możesz śledzić postępy, klikając maszynę wirtualną i otwierając jej właściwości lub przechodząc do zadania **Testowy tryb failover** w pozycji **Ustawienia** > **Zadania usługi Site Recovery**.
1. Po zakończeniu trybu failover w pozycji Azure Portal > **Maszyny wirtualne** powinna być widoczna replika maszyny wirtualnej platformy Azure. Upewnij się, że maszyna wirtualna ma prawidłowy rozmiar oraz że została podłączona do odpowiedniej sieci i uruchomiona.
1. Jeśli przeprowadzono [przygotowanie do nawiązywania połączeń po przejściu do trybu failover](#prepare-to-connect-to-Azure-VMs-after-failover), powinno być możliwe nawiązanie połączenia z maszyną wirtualną Azure.
1. Po zakończeniu kliknij pozycję **Wyczyść test pracy w trybie failover** w planie odzyskiwania. W obszarze **Uwagi** zarejestruj i zapisz wszelkie obserwacje związane z testem pracy w trybie failover. Spowoduje to usunięcie maszyn wirtualnych utworzonych podczas testu pracy w trybie failover. 

Więcej szczegółów można znaleźć w dokumencie [Test failover to Azure](site-recovery-test-failover-to-azure.md) (Testowanie pracy w trybie failover na platformie Azure).

## <a name="monitor-your-deployment"></a>Monitorowanie wdrożenia
Oto, jak można monitorować ustawienia konfiguracji, stan i kondycję wdrożenia usługi Site Recovery:

1. Kliknij nazwę magazynu, aby uzyskać dostęp do pulpitu nawigacyjnego **Podstawy**. W tym pulpicie nawigacyjnym możesz wyświetlić zadania usługi Site Recovery, stan replikacji, plany odzyskiwania, kondycję serwera i zdarzenia.  Możesz dostosować pulpit **Podstawy**, aby pokazywał najbardziej przydatne kafelki i układy, w tym stan innych magazynów usług Site Recovery i Backup.

    ![Podstawy](./media/site-recovery-vmm-to-azure/essentials.png)
2. W kafelku **Kondycja** możesz monitorować problemy związane z serwerami lokacji (serwerami programu VMM lub konfiguracji) oraz zdarzenia wygenerowane przez usługę Site Recovery w ciągu ostatnich 24 godzin.
3. Korzystając z kafelków **Elementy replikowane**, **Plany odzyskiwania** i **Zadania usługi Site Recovery**, możesz zarządzać replikacją i monitorować ją. Możesz przejść do szczegółów zadań w pozycji **Ustawienia** > **Zadania** > **Zadania usługi Site Recovery**.

## <a name="next-steps"></a>Następne kroki
Po skonfigurowaniu i uruchomieniu wdrożenia [dowiedz się więcej](site-recovery-failover.md) o różnych typach trybu failover.



<!--HONumber=Jan17_HO5-->


