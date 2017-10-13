---
title: Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM do platformy Azure | Microsoft Docs
description: "Aranżowanie replikacji, pracy w trybie failover i odzyskiwania maszyn wirtualnych funkcji Hyper-V zarządzanych w chmurach programu System Center VMM do platformy Azure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: 958b61f5de732a882e0a2682b8dd4e18504a6ae7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-site-recovery-in-the-azure-portal"></a>Replikowanie maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM do platformy Azure przy użyciu usługi Site Recovery w witrynie Azure Portal
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [Klasyczny portal Azure](site-recovery-vmm-to-azure-classic.md)
> * [Program PowerShell — model usługi Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [PowerShell — model klasyczny](site-recovery-deploy-with-powershell.md)


W tym artykule opisano sposób replikowania lokalnych maszyn wirtualnych funkcji Hyper-V zarządzanych w chmurach programu System Center VMM do platformy Azure przy użyciu usługi [Azure Site Recovery](site-recovery-overview.md) w witrynie Azure Portal.

Po przeczytaniu tego artykułu zamieść wszelkie komentarze u dołu lub na [forum usług Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

Jeśli chcesz migrować maszyny na platformę Azure (bez powrotu po awarii), zapoznaj się z [tym artykułem](site-recovery-migrate-to-azure.md).


## <a name="deployment-steps"></a>Kroki wdrażania

Postępuj zgodnie z instrukcjami w tym artykule, aby wykonać te kroki wdrażania:


1. [Dowiedz się więcej](site-recovery-components.md) o architekturze do tego wdrożenia. [Dowiedz się](site-recovery-hyper-v-azure-architecture.md) też, jak działa replikacja funkcji Hyper-V w usłudze Site Recovery.
2. Zweryfikuj wymagania wstępne i ograniczenia.
3. Skonfiguruj konta magazynu i sieć platformy Azure.
4. Przygotuj lokalny serwer VMM i hosty funkcji Hyper-V.
5. Utwórz magazyn usługi Recovery Services. Magazyn zawiera ustawienia konfiguracji i aranżuje replikację.
6. Określ ustawienia źródła. Zarejestruj serwer VMM w magazynie. Zainstaluj dostawcę usługi Azure Site Recovery na serwerze VMM oraz agenta usługi Microsoft Recovery Services na hostach funkcji Hyper-V.
7. Skonfiguruj ustawienia replikacji i miejsca docelowe.
8. Włącz replikację maszyn wirtualnych.
9. Uruchom testowanie trybu failover, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami.



## <a name="prerequisites"></a>Wymagania wstępne


**Wymaganie obsługi** | **Szczegóły**
--- | ---
**Azure** | Dowiedz się więcej na temat [wymagań platformy Azure](site-recovery-prereq.md#azure-requirements).
**Serwery lokalne** | [Dowiedz się więcej](site-recovery-prereq.md#disaster-recovery-of-hyper-v-vms-in-vmm-clouds-to-azure) na temat wymagań dotyczących lokalnego serwera VMM i hostów funkcji Hyper-V.
**Lokalne maszyny wirtualne funkcji Hyper-V** | Maszyny wirtualne, które mają być replikowane, powinny mieć [obsługiwane systemy operacyjne](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) i spełniać [wymagania wstępne platformy Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Adresy URL platformy Azure** | Serwer VMM musi mieć dostęp do następujących adresów URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Jeśli masz reguły zapory oparte na adresie IP, zadbaj o to, aby reguły zezwalały na komunikację z platformą Azure.<br/></br> Zezwól na użycie [zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) oraz portu 443 protokołu HTTPS.<br/></br> Zezwól na użycie zakresów adresów IP dla regionu platformy Azure Twojej subskrypcji oraz regionu Zachodnie stany USA (służy do kontrolowania dostępu i zarządzania tożsamościami).


## <a name="prepare-for-deployment"></a>Przygotowanie do wdrożenia
Aby przygotować się do wdrożenia, należy wykonać następujące czynności:

1. [Skonfiguruj sieć platformy Azure](#set-up-an-azure-network), w której maszyny wirtualne Azure zostaną umieszczone po przejściu do trybu failover.
2. [Skonfiguruj konto usługi Azure Storage](#set-up-an-azure-storage-account) dla replikowanych danych.
3. [Przygotuj serwer programu VMM](#prepare-the-vmm-server) na wdrożenie usługi Site Recovery.
4. Przygotuj się do mapowania sieci. Skonfiguruj sieci, aby umożliwić skonfigurowanie mapowania sieci podczas wdrażania usługi Site Recovery.

### <a name="set-up-an-azure-network"></a>Konfiguracja sieci platformy Azure
Potrzebujesz sieci platformy Azure, z którą będą się łączyć maszyny wirtualne platformy Azure utworzone po przejściu do trybu failover.

* Sieć powinna znajdować się w tym samym regionie co magazyn usługi Recovery Services.
* W zależności od modelu zasobu, który chcesz wykorzystać dla maszyn wirtualnych platformy Azure w trybie failover, musisz skonfigurować sieć platformy Azure w [modelu usługi Resource Manager](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) lub [modelu klasycznym](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).
* Zaleca się skonfigurowanie sieci przed rozpoczęciem dalszych działań. Jeśli tego nie zrobisz, konfigurację będzie trzeba przeprowadzić podczas wdrażania usługi Site Recovery.
Sieci platformy Azure używanych na potrzeby usługi Site Recovery nie można [przenosić](../azure-resource-manager/resource-group-move-resources.md) w ramach tej samej ani różnych subskrypcji.

### <a name="set-up-an-azure-storage-account"></a>Konfigurowanie konta usługi Azure Storage
* Potrzebujesz konta standardowego lub konta Premium usługi Azure Storage do przechowywania danych replikowanych do platformy Azure. [Usługa Premium Storage](../storage/common/storage-premium-storage.md) jest przeznaczona dla maszyn wirtualnych wymagających spójnej wysokiej wydajności we/wy oraz małych opóźnień w celu hostowania obciążeń intensywnie wykorzystujących operacje we/wy. Jeśli używasz konta Premium do przechowywania replikowanych danych, potrzebujesz też standardowego konta magazynu do przechowywania dzienników replikacji, które przechwytują zachodzące zmiany w danych lokalnych. Konto musi znajdować się w tym samym regionie co magazyn Usług odzyskiwania.
* W zależności od modelu zasobu, który chcesz wykorzystać dla maszyn wirtualnych Azure w trybie failover, należy skonfigurować konto w [modelu Resource Manager](../storage/common/storage-create-storage-account.md) lub [modelu klasycznym](../storage/common/storage-create-storage-account.md).
* Zalecamy skonfigurowanie konta przed rozpoczęciem dalszych działań. Jeśli tego nie zrobisz, konfigurację będzie trzeba przeprowadzić podczas wdrażania usługi Site Recovery.
- Należy pamiętać, że kont magazynu używanych przez usługę Site Recovery nie można [przenosić](../azure-resource-manager/resource-group-move-resources.md) w ramach tej samej lub różnych subskrypcji.

### <a name="prepare-the-vmm-server"></a>Przygotowanie serwera programu VMM
* Upewnij się, że serwer programu VMM jest zgodny z [wymaganiami wstępnymi](#prerequisites).
* Podczas wdrażania usługi Site Recovery możesz określić, że wszystkie chmury na serwerze VMM powinny być dostępne w portalu Azure. Jeśli chcesz, aby tylko wybrane chmury były dostępne w portalu, możesz włączyć takie ustawienie dla chmury w konsoli administracyjnej programu VMM.

### <a name="prepare-for-network-mapping"></a>Przygotowanie do mapowania sieci
Podczas wdrażania usługi Site Recovery musisz skonfigurować mapowanie sieci. Mapowanie sieci działa między źródłowymi sieciami maszyny wirtualnej VMM i docelowymi sieciami platformy Azure, aby umożliwić korzystanie z następujących funkcji:

* Maszyny, które są przełączane w tryb failover w tej samej sieci, mogą się ze sobą łączyć, nawet jeśli przeszły do trybu failover na różne sposoby lub w ramach innych planów odzyskiwania.
* Jeśli brama sieci została skonfigurowana w docelowej sieci platformy Azure, maszyny wirtualne Azure mogą łączyć się z lokalnymi maszynami wirtualnymi.
* Aby skonfigurować mapowanie sieci, należy wykonać następujące czynności:

  * Upewnij się, że maszyny wirtualne na źródłowym serwerze hosta funkcji Hyper-V są podłączone do sieci maszyny wirtualnej VMM. Ta sieć powinna być połączona z siecią logiczną skojarzoną z chmurą.
  * Sieć platformy Azure zgodnie z [powyższym](#set-up-an-azure-network) opisem

## <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu Usług odzyskiwania
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Kliknij pozycję **Nowy** > **Monitorowanie i zarządzanie** > **Backup and Site Recovery (OMS)**.

    ![Nowy magazyn](./media/site-recovery-vmm-to-azure/new-vault3.png)
3. W polu **Nazwa** wprowadź przyjazną nazwę identyfikującą magazyn. Jeśli masz więcej niż jedną subskrypcję, wybierz jedną z nich.
4. [Utwórz grupę zasobów](../azure-resource-manager/resource-group-template-deploy-portal.md) lub wybierz istniejącą. Określ region platformy Azure. Maszyny będą replikowane do tego regionu. Aby sprawdzić obsługiwane regiony, zobacz sekcję dotyczącą dostępności geograficznej w temacie [Szczegóły cennika usługi Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/)
5. Jeśli chcesz szybko uzyskać dostęp do magazynu z pulpitu nawigacyjnego, kliknij pozycję **Przypnij do pulpitu nawigacyjnego** > **Utwórz magazyn**.

    ![Nowy magazyn](./media/site-recovery-vmm-to-azure/new-vault.png)

Nowy magazyn będzie wyświetlany w sekcji **Pulpit nawigacyjny** > **Wszystkie zasoby** oraz w głównym bloku **magazynów usług Recovery Services**.


## <a name="select-the-protection-goal"></a>Wybieranie celu ochrony

Wybierz, co chcesz replikować, i miejsce, do którego chcesz przeprowadzać replikację.

1. W obszarze **Magazyny usługi Recovery Services** wybierz magazyn.
2. W sekcji **Wprowadzenie** kliknij pozycję **Site Recovery** > **Przygotowanie infrastruktury** > **Cel ochrony**.

    ![Wybieranie celów](./media/site-recovery-vmm-to-azure/choose-goals.png)
3. W obszarze **Cel ochrony** wybierz pozycję **Do platformy Azure** i wybierz opcję **Tak, z funkcją Hyper-V**. Wybierz opcję **Tak**, aby potwierdzić, że używasz programu VMM do zarządzania hostami funkcji Hyper-V i lokacją odzyskiwania. Następnie kliknij przycisk **OK**.

## <a name="set-up-the-source-environment"></a>Konfigurowanie środowiska źródłowego

Zainstaluj dostawcę usługi Azure Site Recovery na serwerze programu VMM i zarejestruj serwer w magazynie. Zainstaluj agenta Usług odzyskiwania Azure na hostach funkcji Hyper-V.

1. Kliknij pozycję **Przygotowanie infrastruktury** > **Źródło**.

    ![Konfiguracja źródła](./media/site-recovery-vmm-to-azure/set-source1.png)

2. W obszarze **Przygotowywanie źródła** kliknij pozycję **+ VMM**, aby dodać serwer programu VMM.

    ![Konfiguracja źródła](./media/site-recovery-vmm-to-azure/set-source2.png)

3. W obszarze **Dodawanie serwera** sprawdź, czy **Serwer programu System Center VMM** pojawia się w sekcji **Typ serwera** i czy serwer VMM spełnia [warunki wstępne i wymagania dotyczące adresu URL](#prerequisites).
4. Pobierz plik instalacyjny dostawcy usługi Azure Site Recovery.
5. Pobierz klucz rejestracji. Będzie on potrzebny po uruchomieniu Instalatora. Klucz jest ważny przez pięć dni po jego wygenerowaniu.

    ![Konfiguracja źródła](./media/site-recovery-vmm-to-azure/set-source3.png)


## <a name="install-the-provider-on-the-vmm-server"></a>Instalowanie dostawcy na serwerze VMM

1. Uruchom plik Instalatora dostawcy na serwerze VMM.
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

    > [!NOTE]
    > Zaleca się szyfrowanie danych magazynowanych za pomocą możliwości szyfrowania udostępnianej przez platformę Azure, a nie przy użyciu opcji szyfrowania danych oferowanej przez usługę Azure Site Recovery. Możliwość szyfrowania udostępniana przez platformę Azure może zostać włączona dla magazynu > konta i pomaga osiągnąć lepszą wydajność, ponieważ szyfrowanie i odszyfrowywanie jest obsługiwane przez usługę Azure Storage.
    > [Dowiedz się więcej o funkcji Szyfrowanie usługi Storage platformy Azure](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption).
    
8. W polu **Nazwa serwera** wprowadź przyjazną nazwę identyfikującą serwer VMM w magazynie. W konfiguracji klastra określ nazwę roli klastra VMM.
9. Włącz **synchronizację metadanych chmury**, jeśli chcesz synchronizować metadane dla wszystkich chmur na serwerze VMM w magazynie. To działanie ma miejsce tylko raz na każdym serwerze. Jeśli nie chcesz synchronizować wszystkich chmur, możesz nie zaznaczać tego ustawienia i synchronizować poszczególne chmury indywidualnie we właściwościach chmury w konsoli programu VMM. Kliknij przycisk **Zarejestruj**, aby zakończyć proces.

    ![Rejestracja serwera](./media/site-recovery-vmm-to-azure/provider16.PNG)
10. Rozpoczyna się rejestracja. Po zakończeniu rejestracji serwer jest wyświetlany w obszarze **Infrastruktura usługi Site Recovery** > **Serwery VMM**.


## <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a>Instalacja agenta Usług odzyskiwania Azure na hostach funkcji Hyper-V

1. Po skonfigurowaniu dostawcy należy pobrać plik instalacyjny agenta usług Azure Recovery Services. Uruchom Instalator na każdym serwerze funkcji Hyper-V w chmurze VMM.

    ![Lokacje funkcji Hyper-V](./media/site-recovery-vmm-to-azure/hyperv-agent1.png)
2. W obszarze **Sprawdzanie wymagań wstępnych** kliknij przycisk **Dalej**. Wszystkie brakujące wymagania wstępne zostaną zainstalowane automatycznie.

    ![Wymagania wstępne dotyczące agenta Usług odzyskiwania](./media/site-recovery-vmm-to-azure/hyperv-agent2.png)
3. W obszarze **Ustawienia instalacji** zaakceptuj lub zmodyfikuj lokalizację instalacji i lokalizację pamięci podręcznej. Możesz skonfigurować pamięć podręczną na dysku, który ma co najmniej 5 GB dostępnego miejsca, ale zalecamy dysk pamięci podręcznej z co najmniej 600 GB wolnego miejsca. Następnie kliknij pozycję **Zainstaluj**.
4. Po zakończeniu instalacji kliknij przycisk **Zamknij**, aby zakończyć.

    ![Rejestracja agenta MARS](./media/site-recovery-vmm-to-azure/hyperv-agent3.png)

### <a name="command-line-installation"></a>Instalacja przy użyciu wiersza polecenia
Agenta Usług odzyskiwania Microsoft Azure można zainstalować z poziomu wiersza polecenia przy użyciu następującego polecenia:

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a>Konfigurowanie internetowego dostępu serwera proxy do usługi Site Recovery na hostach funkcji Hyper-V

Agent Usług odzyskiwania uruchomiony na hostach funkcji Hyper-V wymaga internetowego dostępu do platformy Azure, aby umożliwić replikację maszyny wirtualnej. Jeśli uzyskujesz dostęp do Internetu za pośrednictwem serwera proxy, skonfiguruj go w następujący sposób:

1. Otwórz przystawkę MMC usługi Microsoft Azure Backup na hoście funkcji Hyper-V. Domyślnie skrót do usługi Microsoft Azure Backup jest dostępny na pulpicie lub w folderze C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. W przystawce kliknij pozycję **Zmień właściwości**.
3. Na karcie **Konfiguracja serwera proxy** podaj informacje dotyczące serwera proxy.

    ![Rejestracja agenta MARS](./media/site-recovery-vmm-to-azure/mars-proxy.png)
4. Upewnij się, że agent może osiągnąć adresy URL opisane w [wymaganiach wstępnych](#on-premises-prerequisites).

## <a name="set-up-the-target-environment"></a>Konfigurowanie środowiska docelowego
Określ konto magazynu Azure do wykorzystania podczas replikacji i sieć platformy Azure, z którą maszyny wirtualne Azure będą się łączyć po przejściu w tryb failover.

1. Kliknij pozycję **Przygotowywanie infrastruktury** > **Cel**, a następnie wybierz subskrypcję i grupę zasobów, w której chcesz utworzyć maszyny wirtualne w trybie failover. Wybierz model wdrażania, którego chcesz użyć na platformie Azure (model usługi Resource Manager lub model klasyczny) dla maszyn wirtualnych w trybie failover.

    ![Magazyn](./media/site-recovery-vmm-to-azure/enablerep3.png)

2. Usługa Site Recovery sprawdza, czy masz co najmniej jedno zgodne konto magazynu Azure i co najmniej jedną sieć platformy Azure.

    ![Magazyn](./media/site-recovery-vmm-to-azure/compatible-storage.png)

3. Jeśli nie utworzono konta magazynu, a chcesz je utworzyć przy użyciu usługi Resource Manager, kliknij pozycję **+ Konto magazynu**, aby zrobić to w tym miejscu.  W bloku **Utwórz konto magazynu** określ nazwę konta, jego typ, subskrypcję i lokalizację. Konto powinno znajdować się w tej samej lokalizacji co magazyn Usług odzyskiwania.

   ![Magazyn](./media/site-recovery-vmm-to-azure/gs-createstorage.png)


   * Jeśli chcesz utworzyć konto magazynu przy użyciu modelu klasycznego, należy to zrobić w witrynie Azure Portal. [Dowiedz się więcej](../storage/common/storage-create-storage-account.md)
   * Jeśli używasz konta usługi Premium Storage dla replikowanych danych, skonfiguruj dodatkowe, standardowe konto magazynu do przechowywania dzienników replikacji, które przechwytują zachodzące zmiany w danych lokalnych.
5. Jeśli nie utworzono sieci platformy Azure, a chcesz ją utworzyć przy użyciu usługi Resource Manager, kliknij pozycję **+ Sieć**, aby zrobić to w tym miejscu. W bloku **Utwórz sieć wirtualną** określ nazwę sieciową, zakres adresów, szczegóły podsieci, subskrypcję i lokalizację. Sieć powinna znajdować się w tej samej lokalizacji co magazyn Usług odzyskiwania.

   ![Sieć](./media/site-recovery-vmm-to-azure/gs-createnetwork.png)

   Jeśli chcesz utworzyć sieć przy użyciu modelu klasycznego, zrób to w witrynie Azure Portal. [Dowiedz się więcej](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

### <a name="configure-network-mapping"></a>Konfiguracja mapowania sieci

* [Przeczytaj](#prepare-for-network-mapping) szybkie omówienie funkcji mapowania sieci.
* Sprawdź, czy maszyny wirtualne na serwerze VMM zostały połączone z siecią maszyny wirtualnej i czy utworzono przynajmniej jedną sieć wirtualną platformy Azure. Wiele sieci maszyn wirtualnych można mapować do pojedynczej sieci platformy Azure.

Skonfiguruj mapowanie w następujący sposób:

1. W obszarze **Infrastruktura usługi Site Recovery** > **Mapowania sieci** > **Mapowanie sieci** kliknij ikonę **+ Mapowanie sieci**.

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

## <a name="configure-replication-settings"></a>Konfigurowanie ustawień replikacji
1. Aby utworzyć nowe zasady replikacji, kliknij kolejno pozycje **Przygotowanie infrastruktury** > **Ustawienia replikacji** > **+ Utwórz i skojarz**.

    ![Sieć](./media/site-recovery-vmm-to-azure/gs-replication.png)
2. W obszarze **Utwórz i skojarz zasady** określ nazwę zasad.
3. W obszarze **Częstotliwość kopiowania** określ, jak często mają być replikowane dane przyrostowe po replikacji początkowej (co 30 sekund, 5 lub 15 minut).

    > [!NOTE]
    >  Częstotliwość 30-sekundowa nie jest obsługiwana w przypadku replikowania do usługi Premium Storage. To ograniczenie wynika z liczby migawek na obiekt blob (100) obsługiwanych przez usługę Premium Storage. [Dowiedz się więcej](../storage/common/storage-premium-storage.md#snapshots-and-copy-blob)

4. W obszarze **Przechowywanie punktu odzyskiwania** określ w godzinach, jak długie będzie okno przechowywania dla każdego punktu odzyskiwania. Chronione maszyny można odzyskać do dowolnego punktu w tym oknie.
5. W obszarze **Częstotliwość migawek spójności aplikacji** określ, jak często (1–12 godzin) będą tworzone punkty odzyskiwania zawierające migawki spójne z aplikacjami. Funkcja Hyper-V wykorzystuje dwa typy migawek — standardową migawkę, która jest przyrostową migawką całej maszyny wirtualnej oraz migawkę spójności aplikacji, która wykonuje migawkę danych aplikacji wewnątrz maszyny wirtualnej w danym punkcie w czasie. Migawki spójne z aplikacjami używają usługi Volume Shadow Copy (VSS), aby zapewnić, że aplikacje są w spójnym stanie podczas wykonywania migawki. Należy pamiętać, że jeśli migawki spójne z aplikacjami zostaną włączone, będzie to miało wpływ na wydajność aplikacji uruchomionych na źródłowych maszynach wirtualnych. Upewnij się, że ustawiona wartość jest mniejsza od liczby skonfigurowanych dodatkowych punktów odzyskiwania.
6. W obszarze **Czas rozpoczęcia replikacji początkowej** określ, kiedy rozpoczyna się replikacja początkowa. Replikacja odbywa się z wykorzystaniem przepustowości Internetu, dlatego warto zaplanować ją poza najbardziej obciążonymi godzinami.
7. W obszarze **Szyfrowanie danych przechowywanych na platformie Azure** określ, czy należy szyfrować dane w stanie spoczynku w usłudze Azure Storage. Następnie kliknij przycisk **OK**.

    ![Zasady replikacji](./media/site-recovery-vmm-to-azure/gs-replication2.png)
8. Jeśli utworzysz nowe zasady, zostaną one automatycznie skojarzone z chmurą VMM. Kliknij przycisk **OK**. Możesz skojarzyć dodatkowe chmury VMM (oraz maszyny wirtualne w tych chmurach) z zasadami replikacji w obszarze **Replikacja** > nazwa_zasad > **Skojarz chmurę VMM**.

    ![Zasady replikacji](./media/site-recovery-vmm-to-azure/policy-associate.png)

## <a name="capacity-planning"></a>Planowanie pojemności

Teraz, po skonfigurowaniu podstawowej infrastruktury, pomyśl o planowaniu pojemności i zorientuj się, czy potrzebujesz dodatkowych zasobów.

Usługa Site Recovery zawiera planistę wydajności, który pomaga w przydzielaniu odpowiednich zasobów do środowiska źródłowego, składników usługi Site Recovery, sieci i magazynu. Planistę można uruchomić w trybie szybkim, aby uzyskać szacunkowe wartości oparte na średnich liczbach maszyn wirtualnych, dysków i pojemności magazynu lub w trybie szczegółowym, w którym należy wprowadzić wartości na poziomie obciążenia. Przed rozpoczęciem:

* Zebrać informacje o środowisku replikacji, w tym o maszynach wirtualnych, liczbie dysków na maszynę wirtualną oraz pojemności dysków.
* Oszacować szybkość codziennych zmian (przenoszenia) dla replikowanych danych. Możesz użyć [planisty wydajności dla replik funkcji Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057), aby to zrobić.

1. Kliknij pozycję **Pobierz**, aby pobrać narzędzie, i uruchom je. [Przeczytaj artykuł](site-recovery-capacity-planner.md) dołączony do narzędzia.
2. Po zakończeniu wybierz opcję **Tak** w polu **Czy uruchamiano planistę wydajności**?

   ![Planowanie pojemności](./media/site-recovery-vmm-to-azure/gs-capacity-planning.png)

   Więcej informacji na temat [sterowania przepustowością sieci](#network-bandwidth-considerations)




## <a name="enable-replication"></a>Włączanie replikacji

Przed rozpoczęciem upewnij się, że konto użytkownika platformy Azure ma wymagane [uprawnienia](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines), aby włączyć replikację nowej maszyny wirtualnej do platformy Azure.

Teraz włącz replikację w następujący sposób:

1. Kliknij kolejno pozycje **Krok 2. Replikowanie aplikacji** > **Źródło**. Po włączeniu replikacji po raz pierwszy kliknij pozycję **+ Replikuj** w magazynie, aby włączyć replikację dla dodatkowych maszyn.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication1.png)
2. W bloku **Źródło** wybierz serwer programu VMM i chmurę, w której znajdują się hosty funkcji Hyper-V. Następnie kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication-source.png)
3. W obszarze **Cel** wybierz subskrypcję, model wdrożenia po przejściu do trybu failover oraz konto magazynu używane do przechowywania replikowanych danych.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication-target.png)
4. Wybierz konto magazynu, którego chcesz użyć. Jeśli chcesz użyć konta magazynu innego niż posiadane, możesz [utworzyć konto](#set-up-an-azure-storage-account). Jeśli używasz konta usługi Premium Storage dla replikowanych danych, musisz skonfigurować dodatkowe, standardowe konto magazynu do przechowywania dzienników replikacji, które przechwytują zachodzące zmiany w danych lokalnych. Aby utworzyć konto magazynu za pomocą modelu usługi Resource Manager, kliknij pozycję **Utwórz nowy**. Jeśli chcesz utworzyć konto magazynu przy użyciu modelu klasycznego, należy to zrobić [w witrynie Azure Portal](../storage/common/storage-create-storage-account.md). Następnie kliknij przycisk **OK**.
5. Wybierz sieć platformy Azure i podsieć, z którą nawiążą połączenie maszyny wirtualne platformy Azure, gdy zostaną uruchomione po przejściu do trybu failover. Wybierz opcję **Konfiguruj teraz dla wybranych maszyn**, aby zastosować ustawienia sieci do wszystkich maszyn wybranych do ochrony. Wybierz opcję **Konfiguruj później**, aby wybrać sieć platformy Azure dla poszczególnych maszyn. Jeśli chcesz użyć sieci innej niż posiadana, możesz [utworzyć sieć](#set-up-an-azure-network). Aby utworzyć sieć przy użyciu modelu Resource Manager, kliknij pozycję **Utwórz nowe**. Jeśli chcesz utworzyć sieć przy użyciu modelu klasycznego, zrób to [w witrynie Azure Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Wybierz podsieć, jeśli jest to konieczne. Następnie kliknij przycisk **OK**.
6. W pozycji **Maszyny wirtualne** > **Wybierz maszyny wirtualne** kliknij i zaznacz każdą maszynę, którą chcesz replikować. Możesz wybrać tylko te maszyny, dla których można włączyć replikację. Następnie kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication5.png)

7. W pozycji **Właściwości** > **Konfiguracja właściwości** wybierz system operacyjny dla wybranych maszyn wirtualnych oraz dysk systemu operacyjnego.

    - Sprawdź, czy nazwa maszyny wirtualnej platformy Azure (nazwa docelowa) jest zgodna z [wymaganiami maszyny wirtualnej platformy Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).   
    - Domyślnie do replikacji są wybierane wszystkie dyski maszyny wirtualnej, ale można wyczyścić pozycje dysków, aby je wykluczyć.

        - Dyski można wykluczyć w celu zmniejszenia obciążenia przepustowości replikacji. Możesz na przykład zrezygnować z replikacji dysków z danymi tymczasowymi lub danych odświeżanych podczas każdego ponownego uruchomienia maszyny lub aplikacji (na przykład pliku pagefile.sys lub tempdb programu Microsoft SQL Server). Dysk można wykluczyć z replikacji, cofając jego zaznaczenie.
        - Wykluczyć można tylko dyski podstawowe. Nie można wykluczyć dysków systemu operacyjnego.
        - Nie zalecamy wykluczania dysków dynamicznych. Wewnątrz maszyny wirtualnej gościa usługa Site Recovery nie może ustalić, czy wirtualny dysk twardy jest dyskiem podstawowym, czy dynamicznym. Jeśli wszystkie zależne dyski woluminu dynamicznego nie zostaną wykluczone, chroniony dysk dynamiczny będzie wskazywany jako uszkodzony dysk maszyny wirtualnej w trybie failover, a dane na tym dysku nie będą dostępne.
        - Po włączeniu replikacji nie można dodawać ani usuwać dysków do replikacji. Jeśli chcesz dodać lub wykluczyć dysk, musisz wyłączyć ochronę maszyny wirtualnej, a następnie włączyć ją ponownie.
        - Dyski tworzone ręcznie na platformie Azure nie mają możliwości powrotu po awarii. Jeśli na przykład przeniesiesz trzy dyski do trybu failover i utworzysz dwa bezpośrednio na maszynie wirtualnej platformy Azure, tylko dla trzech dysków przeniesionych do trybu failover nastąpi powrót po awarii z platformy Azure do funkcji Hyper-V. Dysków tworzonych ręcznie nie można uwzględnić podczas powrotu po awarii ani replikacji odwrotnej z funkcji Hyper-V do platformy Azure.
        - Jeśli wykluczasz dysk wymagany do działania aplikacji, po przejściu do trybu failover na platformie Azure musisz ręcznie utworzyć go na tej platformie, aby można było uruchomić replikowaną aplikację. Można również zintegrować usługę Azure Automation w planie odzyskiwania, aby utworzyć dysk podczas pracy maszyny w trybie failover.

    Kliknij przycisk **OK**, aby zapisać zmiany. Później możesz skonfigurować dodatkowe właściwości.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication6-with-exclude-disk.png)

8. W pozycji **Ustawienia replikacji** > **Konfigurowanie ustawień replikacji** wybierz zasady replikacji, które chcesz zastosować dla chronionych maszyn wirtualnych. Następnie kliknij przycisk **OK**. Możesz zmodyfikować zasady replikacji w pozycji **Zasady replikacji** > nazwa_zasad > **Edytuj ustawienia**. Zastosowane zmiany są używane dla obecnie replikowanych i nowych maszyn.

   ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/enable-replication7.png)

Możesz śledzić postępy zadania **Włącz ochronę** w obszarze **Zadania** > **Zadania usługi Site Recovery**. Po uruchomieniu zadania **Sfinalizuj ochronę** maszyna jest gotowa do przejścia w tryb failover.

### <a name="view-and-manage-vm-properties"></a>Wyświetlanie właściwości maszyny wirtualnej i zarządzanie nimi

Zalecamy zweryfikowanie właściwości maszyny źródłowej. Pamiętaj, że nazwa maszyny wirtualnej Azure powinna być zgodna z [wymaganiami dotyczącymi maszyn wirtualnych Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. W obszarze **Elementy chronione** kliknij pozycję **Elementy replikowane** i wybierz maszynę, aby zobaczyć szczegółowe informacje.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/vm-essentials.png)
2. W obszarze **Właściwości** możesz wyświetlić informacje dotyczące replikacji i trybu failover dla danej maszyny wirtualnej.

    ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/test-failover2.png)
3. W pozycji **Obliczenia i sieć** > **Właściwości obliczeń** możesz określić nazwę i docelowy rozmiar maszyny wirtualnej Azure. Zmodyfikuj nazwę, aby była zgodna z [wymaganiami platformy Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements), jeśli zachodzi taka potrzeba. Możesz również wyświetlić i zmodyfikować informacje dotyczące sieci docelowej, podsieci i adresu IP, które zostaną przypisane do maszyny wirtualnej platformy Azure.
Należy pamiętać, że:

   * Możesz ustawić docelowy adres IP. Jeśli nie podasz adresu, maszyna w trybie failover będzie używać protokołu DHCP. Jeśli ustawisz adres, który nie jest dostępny w trybie failover, przejście do trybu failover zakończy się niepowodzeniem. Ten sam docelowy adres IP może być użyty do testowania trybu failover, jeśli adres jest dostępny w testowej sieci trybu failover.
   * Liczba kart sieciowych zależy od rozmiaru określonego dla docelowej maszyny wirtualnej, zgodnie z poniższym:

     * Jeśli liczba kart sieciowych w maszynie źródłowej jest mniejsza lub równa liczbie kart sieciowych dozwolonych dla rozmiaru maszyny docelowej, maszyna docelowa będzie mieć taką samą liczbę kart sieciowych jak maszyna źródłowa.
     * Jeśli liczba kart sieciowych dla źródłowej maszyny wirtualnej przekracza liczbę dozwolonych kart sieciowych dla rozmiaru maszyny docelowej, zostanie użyta maksymalna liczba kart dla rozmiaru maszyny docelowej.
     * Przykładowo, jeśli maszyna źródłowa ma dwie karty sieciowe, a rozmiar maszyny docelowej obsługuje cztery karty, maszyna docelowa będzie mieć dwie karty sieciowe. Jeśli maszyna źródłowa ma dwie karty sieciowe, ale rozmiar maszyny docelowej obsługuje tylko jedną kartę, maszyna docelowa będzie mieć tylko jedną kartę sieciową.     
     * Jeśli maszyna wirtualna ma kilka kart sieciowych, wszystkie będą łączyć się z tą samą siecią.

     ![Włączanie replikacji](./media/site-recovery-vmm-to-azure/test-failover4.png)

4. W pozycji **Dyski** możesz zobaczyć dyski systemu operacyjnego i danych na maszynie wirtualnej, która będzie replikowana.

#### <a name="managed-disks"></a>Dyski zarządzane

W obszarze **Obliczenia i sieć** > **Właściwości obliczania** możesz dla ustawienia Użyj usługi Managed Disks skonfigurować wartość Tak dla maszyny wirtualnej, jeśli chcesz dołączyć dyski zarządzane maszyny do migracji na platformę Azure. Usługa Managed Disks upraszcza zarządzanie dyskami maszyn wirtualnych usługi Azure IaaS dzięki zarządzaniu skojarzonymi z tymi dyskami kontami magazynu. [Dowiedz się więcej o dyskach zarządzanych](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview).

   - Dyski zarządzane są tworzone i dołączane do maszyny wirtualnej tylko w trybie failover na platformie Azure. Po włączeniu ochrony dane z maszyn lokalnych będą nadal replikowane do kont magazynu.
   Dyski zarządzane można tworzyć tylko dla maszyn wirtualnych wdrożonych w modelu wdrożenia opartym na usłudze Resource Manager.  

  > [!NOTE]
  > Powrót po awarii z platformy Azure do lokalnego środowiska funkcji Hyper-V nie jest obecnie obsługiwany w przypadku maszyn z dyskami zarządzanymi. Wartość Tak dla ustawienia Użyj usługi Managed Disks konfiguruj tylko w przypadku planowania migracji danej maszyny na platformę Azure.

   - Po skonfigurowaniu wartości Tak dla ustawienia Użyj usługi Managed Disks do wyboru będą dostępne tylko zestawy dostępności w grupie zasobów z wartością Tak dla ustawienia Użyj usługi Managed Disks. Jest to spowodowane tym, że maszyny wirtualne z dyskami zarządzanymi mogą należeć tylko do zestawów dostępności z ustawieniem Użyj usługi Managed Disks o wartości Tak. Utwórz zestawy dostępności z konfiguracją właściwości Użyj usługi Managed Disks zgodną z planem używania dysków zarządzanych w trybie failover.  Podobnie po skonfigurowaniu wartości Nie dla ustawienia Użyj usługi Managed Disks do wyboru będą dostępne tylko zestawy dostępności w grupie zasobów z wartością Nie dla właściwości Użyj usługi Managed Disks. [Dowiedz się więcej o dyskach zarządzanych i zestawach dostępności](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Jeśli konto magazynu używane do replikacji zostało kiedykolwiek zaszyfrowane przy użyciu funkcji Szyfrowanie usługi Storage, próba utworzenia dysków zarządzanych w trybie failover nie powiedzie się. Możesz skonfigurować wartość Nie dla właściwości Użyj usługi Managed Disks i ponowić próbę włączenia trybu failover lub wyłączyć ochronę maszyny wirtualnej i włączyć jej ochronę za pomocą konta magazynu, dla którego nigdy nie włączono funkcji Szyfrowanie usługi Storage.
  > [Dowiedz się więcej o funkcji Szyfrowanie usługi Storage i dyskach zarządzanych](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption).


## <a name="test-the-deployment"></a>Testowanie wdrożenia

Aby przetestować wdrożenie, możesz uruchomić test trybu failover dla jednej maszyny wirtualnej lub plan odzyskiwania, który zawiera co najmniej jedną maszynę wirtualną.

### <a name="before-you-start"></a>Przed rozpoczęciem

 - Jeśli chcesz nawiązać połączenie z maszynami wirtualnymi platformy Azure przy użyciu protokołu RDP po przejściu do trybu failover, dowiedz się, jak [przygotować się do połączenia](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - Do przeprowadzenia pełnego testowania potrzebna jest kopia usług Active Directory i DNS w środowisku testowym. [Dowiedz się więcej](site-recovery-active-directory.md#test-failover-considerations).

### <a name="run-a-test-failover"></a>Wykonywanie próby przejścia w tryb failover

1. Aby przenieść jedną maszynę wirtualną do trybu failover, w obszarze **Elementy replikowane** kliknij maszynę wirtualną > **+ Testowy tryb failover**.
2. Aby przenieść plan odzyskiwania do trybu failover, w obszarze **Plany odzyskiwania** kliknij plan prawym przyciskiem myszy > **Testowy tryb failover**. Aby utworzyć plan odzyskiwania, [wykonaj te instrukcje](site-recovery-create-recovery-plans.md).
3. W pozycji **Testowy tryb failover** wybierz sieć platformy Azure, z którą maszyny wirtualne Azure zostaną połączone po przejściu do trybu failover.
4. Kliknij przycisk **OK**, aby rozpocząć tryb failover. Możesz śledzić postępy, klikając maszynę wirtualną i otwierając jej właściwości lub przechodząc do zadania **Testowy tryb failover** w obszarze **Zadania usługi Site Recovery**.
5. Po zakończeniu trybu failover w pozycji Azure Portal > **Maszyny wirtualne** powinna być widoczna replika maszyny wirtualnej platformy Azure. Upewnij się, że maszyna wirtualna ma prawidłowy rozmiar oraz że została podłączona do odpowiedniej sieci i uruchomiona.
6. Jeśli przeprowadzono przygotowanie do nawiązywania połączeń po przejściu do trybu failover, powinno być możliwe nawiązanie połączenia z maszyną wirtualną platformy Azure.
7. Po zakończeniu kliknij pozycję **Wyczyść test pracy w trybie failover** w planie odzyskiwania. W obszarze **Uwagi** zarejestruj i zapisz wszelkie obserwacje związane z testem pracy w trybie failover. Spowoduje to usunięcie maszyn wirtualnych utworzonych podczas testu pracy w trybie failover.

Więcej szczegółów można znaleźć w dokumencie [Testowanie pracy w trybie failover na platformie Azure](site-recovery-test-failover-to-azure.md).

## <a name="monitor-the-deployment"></a>Monitorowanie wdrożenia

Oto jak można monitorować ustawienia konfiguracji, stan i kondycję wdrożenia usługi Site Recovery:

1. Kliknij nazwę magazynu, aby uzyskać dostęp do pulpitu nawigacyjnego **Podstawy**. W tym pulpicie nawigacyjnym możesz wyświetlić zadania usługi Site Recovery, stan replikacji, plany odzyskiwania, kondycję serwera i zdarzenia.  Możesz dostosować pulpit **Podstawy**, aby pokazywał najbardziej przydatne kafelki i układy, w tym stan innych magazynów usług Site Recovery i Backup.

    ![Podstawy](./media/site-recovery-vmm-to-azure/essentials.png)
2. W obszarze **Kondycja** możesz monitorować problemy związane z serwerami lokalnymi (serwerami programu VMM lub konfiguracji) oraz zdarzenia wygenerowane przez usługę Site Recovery w ciągu ostatnich 24 godzin.
3. Korzystając z kafelków **Elementy replikowane**, **Plany odzyskiwania** i **Zadania usługi Site Recovery**, możesz zarządzać replikacją i monitorować ją. Możesz przejść do szczegółów zadań w pozycji **Zadania** > **Zadania usługi Site Recovery**.

## <a name="command-line-installation-for-the-azure-site-recovery-provider"></a>Instalacja dostawcy usługi Azure Site Recovery przy użyciu wiersza polecenia

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

    > [!NOTE]
    > Zaleca się szyfrowanie przechowywanych danych magazynowanych za pomocą możliwości szyfrowania udostępnianej przez platformę Azure, a nie przy użyciu opcji szyfrowania (opcji EncryptionEnabled) udostępnianej przez usługę Azure Site Recovery. Możliwość szyfrowania udostępniana przez platformę Azure może zostać włączona dla konta magazynu i pomaga osiągnąć lepszą wydajność, ponieważ szyfrowanie/odszyfrowywanie jest wykonywane przez usługę Azure  
    > Storage.
    > [Dowiedz się więcej o funkcji Szyfrowanie usługi Storage na platformie Azure](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption).
    
* **/proxyAddress**: opcjonalny parametr określający adres serwera proxy.
* **/proxyport**: opcjonalny parametr określający port serwera proxy.
* **/proxyUsername**: opcjonalny parametr określający nazwę użytkownika serwera proxy (jeśli serwer proxy wymaga uwierzytelniania).
* **/proxyPassword**: jest opcjonalnym parametrem, który określa hasło do uwierzytelniania przy użyciu serwera proxy (jeśli serwer proxy wymaga uwierzytelniania).


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

## <a name="next-steps"></a>Następne kroki

Po zakończeniu replikacji wstępnej i przetestowaniu wdrożenia w razie potrzeby można wywoływać przechodzenie w tryb failover. [Dowiedz się więcej](site-recovery-failover.md) o różnych typach trybu failover i sposobach ich uruchamiania.
