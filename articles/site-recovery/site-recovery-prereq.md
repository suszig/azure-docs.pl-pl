---
title: "Wymagania wstępne dotyczące replikacji na platformie Azure przy użyciu usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat wymagań wstępnych dotyczących replikację maszyn wirtualnych i maszyn fizycznych do platformy Azure przy użyciu usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: jwhit
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/19/2017
ms.author: raynew
ms.openlocfilehash: cb7c3892dc0acbffdec636e5e7b3ce063660153c
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2017
---
#  <a name="prerequisites-for-replication-from-on-premises-to-azure-by-using-site-recovery"></a>Wymagania wstępne dotyczące replikacji z lokalnej na platformie Azure przy użyciu usługi Site Recovery

> [!div class="op_single_selector"]
> * [Replikacja z platformy Azure do platformy Azure](site-recovery-azure-to-azure-prereq.md)
> * [Replikacja z lokalnego do platformy Azure](site-recovery-prereq.md)

Usługa Azure Site Recovery może pomóc obsługuje danej ciągłość prowadzenia działalności biznesowej i strategia odzyskiwania (BCDR) poprzez organizowanie replikacji maszyny wirtualnej platformy Azure (VM) do innego regionu systemu Azure. Usługa Site Recovery również replikacji lokalnych serwerów fizycznych i maszyn wirtualnych do chmury (Azure) lub dodatkowego centrum danych. W przypadku wystąpienia awarii w lokalizacji głównej, możesz w trybie Failover do dodatkowej lokalizacji do zachowania aplikacji i obciążeń dostępne. Użytkownik może zakończyć się niepowodzeniem do lokalizacji głównej lokalizacji głównej zwrócona do normalnego działania. Aby uzyskać więcej informacji na temat odzyskiwania lokacji, zobacz [co to jest usługa Site Recovery?](site-recovery-overview.md).

W tym artykule firma Microsoft Podsumowanie wymagań wstępnych dotyczących rozpoczęciem replikacji usługi Site Recovery na maszynie lokalnej na platformie Azure.

Możesz zamieścić wszelkie komentarze w dolnej części tego artykułu. Ponadto można zadawać pytania techniczne w [forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="azure-requirements"></a>Wymagania systemu Azure

**Wymaganie** | **Szczegóły**
--- | ---
**Konto platformy Azure** | A [konta Microsoft Azure](http://azure.microsoft.com/). Możesz rozpocząć od [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
**Usługa Site Recovery** | Aby uzyskać więcej informacji na temat cen usługi Azure Site Recovery, zobacz [cenach usługi Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/). |
**Azure Storage** | Musisz mieć konto magazynu Azure do przechowywania replikowanych danych. Konto magazynu musi znajdować się w tym samym regionie co magazyn usługi Azure Recovery Services. Maszyny wirtualne platformy Azure są tworzone podczas pracy awaryjnej.<br/><br/> W zależności od modelu zasobu, którego chcesz użyć dla trybu failover maszyny Wirtualnej platformy Azure, można skonfigurować konto za pomocą [modelu wdrażania usługi Azure Resource Manager](../storage/common/storage-create-storage-account.md) lub [klasycznego modelu wdrażania](../storage/common/storage-create-storage-account.md).<br/><br/>Możesz używać [magazynu geograficznie nadmiarowego](../storage/common/storage-redundancy.md#geo-redundant-storage) lub magazynu lokalnie nadmiarowego. Zalecamy magazyn geograficznie nadmiarowy. Dzięki magazynowi geograficznie nadmiarowemu dane będą odporne w przypadku regionalnej awarii lub sytuacji, w której nie można odzyskać regionu podstawowego.<br/><br/> Można używać konta standardowe magazynu Azure, lub skorzystać z usługi Azure Premium Storage. [Magazyn w warstwie Premium](https://docs.microsoft.com/azure/storage/storage-premium-storage) zazwyczaj jest używana w przypadku maszyn wirtualnych, które wymagają wysoką wydajność We/Wy i małe opóźnienia. Z magazyn w warstwie premium maszyna wirtualna może obsługiwać/O wykonujących obciążeń. Jeśli używasz usługi Premium Storage do przechowywania replikowanych danych, potrzebujesz też standardowego konta magazynu. Standardowe konto magazynu służy do przechowywania dzienników replikacji, które przechwytują zachodzące zmiany w danych lokalnych.<br/><br/>
**Ograniczenia magazynu** | Nie można przenieść kont magazynu, które używane w usłudze Site Recovery do innej grupie zasobów, lub Przenieś lub za pomocą innej subskrypcji.<br/><br/> Obecnie replikowanych do kont magazynu premium Indie środkowe i Południowa, Indie nie jest dostępna.
**Sieć platformy Azure** | Potrzebujesz sieci platformy Azure, z którym maszyny wirtualne Azure się łączyć po pracy awaryjnej. Sieć platformy Azure musi znajdować się w tym samym regionie co magazyn usługi Recovery Services.<br/><br/> W portalu Azure, można utworzyć sieci platformy Azure przy użyciu [modelu wdrażania usługi Resource Manager](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) lub [klasycznego modelu wdrażania](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).<br/><br/> Jeśli replikujesz z System Center Virtual Machine Manager (VMM) na platformie Azure, należy skonfigurować mapowanie sieci między sieciami maszyn wirtualnych VMM i sieci platformy Azure. Dzięki temu można połączyć maszyny wirtualne platformy Azure do odpowiedniej sieci po pracy awaryjnej.
**Ograniczenia dotyczące sieci** | Nie można przenieść kont sieci, które używane w usłudze Site Recovery do innej grupie zasobów, lub Przenieś lub za pomocą innej subskrypcji.
**Mapowanie sieci** | Jeśli replikujesz maszyny wirtualne funkcji Hyper-V firmy Microsoft w chmurach programu VMM, należy skonfigurować mapowanie sieci. Dzięki temu, czy maszynach wirtualnych platformy Azure nawiązać odpowiednie sieci, gdy są tworzone po pracy awaryjnej.

> [!NOTE]
> W poniższych sekcjach opisano wymagania wstępne dotyczące różnych składników środowiska klienta. Aby uzyskać więcej informacji na temat obsługi dla określonej konfiguracji, zobacz [macierz obsługi](site-recovery-support-matrix.md).
>

## <a name="disaster-recovery-of-vmware-vms-or-physical-windows-or-linux-servers-to-azure"></a>Odzyskiwanie po awarii maszyn wirtualnych VMware lub serwerach fizycznych systemu Windows lub Linux na platformie Azure
Następujące składniki są wymagane podczas odzyskiwania po awarii maszyn wirtualnych VMware lub serwerach fizycznych systemu Windows lub Linux. Są to oprócz firma opisana w [wymagania dotyczące usługi Azure](#azure-requirements).


### <a name="configuration-server-or-additional-process-server"></a>Konfiguracja serwera lub serwera dodatkowych procesów

Konfigurowanie komputera lokalnego jako serwer konfiguracji do organizowania komunikacji między lokacją lokalną i platformą Azure. Poniżej tabeli omówiono wymagania systemu i oprogramowania skonfigurowanego jako serwer konfiguracji maszyny wirtualnej.

> [!IMPORTANT]
> Podczas wdrażania serwera konfiguracji ochrony maszyn wirtualnych VMware, zaleca się wdrożyć go jako **wysokiej dostępności (HA)** maszyny wirtualnej.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

### <a name="vmware-vcenter-or-vsphere-host-prerequisites"></a>VMware vCenter lub vSphere wymagania wstępne dotyczące hosta

| **Składnik** | **Wymagania** |
| --- | --- |
| **vSphere** | Musi mieć co najmniej jeden VMware funkcji hypervisor vSphere.<br/><br/>Funkcji hypervisor musi być uruchomiona vSphere w wersji 6.0, 5.5 lub 5.1 z najnowszymi aktualizacjami.<br/><br/>Firma Microsoft zaleca tego vSphere hostów i serwerów vCenter się znajdować w tej samej sieci co serwer przetwarzania. Jeśli nie skonfigurowano serwera dedykowanego procesu jest sieci, w którym znajduje się serwer konfiguracji. |
| **vCenter** | Zaleca się wdrożenie serwera VMware vCenter do zarządzania hostach vSphere. Musi ona być uruchomiona vCenter w wersji 6.0 lub 5.5, najnowsze aktualizacje.<br/><br/>**Ograniczenie**: Usługa Site Recovery nie obsługuje replikacji między wystąpieniami vCenter vMotion. Magazyn usługi rejestracji urządzeń i Storage vMotion również nie są obsługiwane w głównym celu maszyny Wirtualnej po zakończeniu operacji ponownej ochrony.|

### <a name="replicated-machine-prerequisites"></a>Wymagania wstępne dotyczące replikowane maszyny

| **Składnik** | **Wymagania** |
| --- | --- |
| **Maszyny lokalne** (maszyny wirtualne VMware) | Replikowane maszyny wirtualne muszą być narzędzia VMware zainstalowany i uruchomiony.<br/><br/> Maszyny wirtualne muszą spełniać [wymagania wstępne platformy Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) do tworzenia maszyny Wirtualnej platformy Azure.<br/><br/>Pojemność dysku na każdym chronionym komputerze nie może być więcej niż 1,023 GB. <br/><br/>Minimalna 2 GB wolnego miejsca na dysku instalacyjnym programu jest wymagane do instalacji składnika.<br/><br/>Jeśli chcesz włączyć spójność wielu maszyn wirtualnych, należy otworzyć port 20004 w zaporze lokalnej maszyny Wirtualnej.<br/><br/>Nazwy komputerów musi wynosić od 1 do 63 znaków (należy użyć litery, cyfry i łączniki). Nazwa musi zaczynać się literą lub cyfrą i kończyć literą lub cyfrą. <br/><br/>Po włączeniu replikacji dla maszyny, można zmodyfikować nazwę platformy Azure.<br/><br/> |
| **Komputery z systemem Windows** (fizycznej lub VMware) | Komputer musi działać jeden z następujących systemów operacyjnych obsługiwanych 64-bitowe: <br/>— Windows Server 2016 (instalacja Server Core, serwer ze środowiskiem pulpitu)<br/>— Windows Server 2012 R2<br/>— Windows Server 2012<br/>— Windows Server 2008 R2 z dodatkiem SP1 lub nowszej wersji.<br/><br/> System operacyjny musi być zainstalowany na dysku C. Dysk systemu operacyjnego musi być dyskiem podstawowym z systemem Windows, a nie dynamiczny. Dysk danych może być dynamiczny.<br/><br/>|
| **Maszyny z systemem Linux** (fizycznej lub VMware) | Komputer musi działać jeden z następujących systemów operacyjnych obsługiwanych 64-bitowe: <br/>-Red Hat Enterprise Linux 5.2-5.11 6.1-6.9, 7.0 i 7.3<br/>-CentOS 5.2-5.11 6.1-6.9, 7.0 i 7.3<br/>-Serwer Ubuntu 14.04 LTS (listę obsługiwanych jądra wersje Ubuntu, zobacz [obsługiwanych systemów operacyjnych](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions))<br/>-Ubuntu 16.04 LTS server (listę obsługiwanych jądra wersje Ubuntu, zobacz [obsługiwanych systemów operacyjnych](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions))<br/>-Debian 7 lub Debian 8<br/>— Oracle Enterprise Linux 6.5 lub 6.4, systemem Red Hat zgodnego jądra lub podzielenie Enterprise jądra wersji 3 (UEK3)<br/>-SUSE Linux Enterprise Server 11 z dodatkiem SP4 lub SUSE Linux Enterprise Server 11 z dodatkiem SP3<br/><br/>Pliki/etc/hosts na chronionych komputerach musi mieć wpisów, które mapują nazwę hosta lokalnego na adresy IP skojarzone z wszystkich kart sieciowych.<br/><br/>Po przejściu w tryb failover Jeśli chcesz połączyć z maszyną wirtualną Azure z systemem Linux przy użyciu klienta Secure Shell (SSH), sprawdź, czy usługa SSH na chronionej maszynie jest ustawiony do automatycznego uruchamiania po uruchomieniu systemu. Upewnij się, również reguły zapory zezwalają na połączenie SSH chronionej maszyny.<br/><br/>Nazwa hosta, punkty instalacji, nazwy urządzenia i ścieżki systemu Linux i nazwy pliku (na przykład/etc / i usr) musi być tylko w języku angielskim.<br/><br/>Następujące katalogi (jeśli skonfigurowany jako osobne partycje lub systemu plików) musi składać się na tym samym dysku (dysk systemu operacyjnego) na serwerze źródłowym:<br/>- / (root)<br/>- / rozruchu<br/>-usr<br/>-/ usr/lokalnego<br/>-/var<br/>- / itp<br/><br/>Obecnie XFS v5 funkcje, takie jak metadanych sumy kontrolnej, nie są obsługiwane przez usługę Site Recovery na XFS systemów plików. Upewnij się, że XFS systemów plików nie są użyciem dowolnej funkcji v5. Narzędzie xfs_info Sprawdź superblock XFS dla partycji. Jeśli **ftype** ustawiono **1**, XFS v5 funkcje są używane.<br/><br/>W przypadku serwerów Red Hat Enterprise Linux 7 i CentOS 7 narzędzie lsof musi być zainstalowany i dostępny.<br/><br/>


## <a name="disaster-recovery-of-hyper-v-vms-to-azure-no-vmm"></a>Odzyskiwanie maszyn wirtualnych funkcji Hyper-V do platformy Azure (bez VMM)

Następujące składniki są wymagane podczas odzyskiwania po awarii maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM. Są to oprócz firma opisana w [wymagania dotyczące usługi Azure](#azure-requirements).

| **Wymagania wstępne** | **Szczegóły** |
| --- | --- |
| **Host funkcji Hyper-V** |Jeden lub więcej serwerów lokalnych musi działać system Windows Server 2012 R2 z najnowsze aktualizacje i włączoną rolę funkcji Hyper-V lub Microsoft Hyper-V Server 2012 R2.<br/><br/>Serwery funkcji Hyper-V musi mieć przynajmniej jednej maszyny wirtualnej.<br/><br/>Serwery funkcji Hyper-V musi połączony z Internetem, bezpośrednio lub za pośrednictwem serwera proxy.<br/><br/>Serwery funkcji Hyper-V muszą mieć poprawki opisanej w artykule bazy wiedzy Knowledge Base [2961977](https://support.microsoft.com/kb/2961977) zainstalowane.
|**Dostawca i agent**| Podczas wdrażania usługi Site Recovery instaluje się dostawcę usługi Azure Site Recovery. Instalacja dostawcy instaluje agenta usług odzyskiwania Azure na każdym serwerze funkcji Hyper-V, na którym jest uruchomiony na maszynach wirtualnych, które chcesz chronić. <br/><br/>Wszystkie serwery funkcji Hyper-V w magazynie usługi Site Recovery muszą mieć takie same jak wersje dostawca i agent.<br/><br/>Dostawca wymaga nawiązania połączenia z usługi Site Recovery za pośrednictwem Internetu. Może zostać wysłany ruch, bezpośrednio lub za pośrednictwem serwera proxy. Serwer proxy oparty na HTTPS nie jest obsługiwane. Serwer proxy musi zezwolić na dostęp do następujących adresów URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/>Jeśli masz reguły zapory oparte na adresie IP na serwerze, należy się upewnić, czy reguły zezwalają na komunikację z platformą Azure.<br/><br/> Zezwalaj na [zakresy IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)i HTTPS (port 443).<br/><br/> Zezwalaj na zakresy adresów IP dla regionu Azure Twojej subskrypcji i zachodnie stany USA (używanych do zarządzania tożsamości i kontroli dostępu).


## <a name="disaster-recovery-of-hyper-v-vms-in-vmm-clouds-to-azure"></a>Odzyskiwanie po awarii maszyn wirtualnych funkcji Hyper-V w chmurach VMM do platformy Azure

Następujące składniki są wymagane podczas odzyskiwania po awarii maszyn wirtualnych funkcji Hyper-V w chmurach programu VMM. Są to oprócz firma opisana w [wymagania dotyczące usługi Azure](#azure-requirements).

| **Wymagania wstępne** | **Szczegóły** |
| --- | --- |
| **Program Virtual Machine Manager** |Musi mieć co najmniej jeden serwer programu VMM uruchomiony w programie System Center 2012 R2 lub nowszy. Każdy serwer VMM musi mieć co najmniej jedną chmurę skonfigurowane. <br/><br/>Chmury muszą mieć:<br/>-Co najmniej jedną grupę hostów programu VMM.<br/>— Serwery hosta funkcji Hyper-V lub klastry w każdej grupie hostów.<br/><br/>Aby uzyskać więcej informacji na temat konfigurowania chmur programu VMM, zobacz [sposobu tworzenia chmury w programie Virtual Machine Manager 2012](http://social.technet.microsoft.com/wiki/contents/articles/2729.how-to-create-a-cloud-in-vmm-2012.aspx). |
| **Funkcja Hyper-V** |Serwery hostów funkcji Hyper-V muszą być uruchomione w systemie Windows Server 2012 R2 z włączoną rolą Hyper-V lub w systemie Microsoft Hyper-V Server 2012 R2. Muszą być zainstalowane najnowsze aktualizacje.<br/><br/> Serwer funkcji Hyper-V musi mieć przynajmniej jednej maszyny wirtualnej.<br/><br/> Serwer hosta funkcji Hyper-V lub klaster, który zawiera maszyny wirtualne, które chcesz replikować muszą być zarządzane w chmurze VMM.<br/><br/>Serwery funkcji Hyper-V musi połączony z Internetem, bezpośrednio lub za pośrednictwem serwera proxy.<br/><br/>Serwery funkcji Hyper-V muszą mieć poprawki opisanej w artykule bazy wiedzy Knowledge Base [2961977](https://support.microsoft.com/kb/2961977) zainstalowane.<br/><br/>Serwery hosta funkcji Hyper-V wymagają dostępu do Internetu do replikacji danych do platformy Azure. |
| **Dostawca i agent** |Podczas wdrażania usługi Azure Site Recovery Zainstaluj dostawcę usługi Azure Site Recovery na serwerze programu VMM. Zainstaluj agenta usług odzyskiwania na hostach funkcji Hyper-V. Dostawca i agent muszą nawiązać Azure bezpośrednio przez Internet lub przez serwer proxy. Serwer proxy oparty na protokole HTTPS nie jest obsługiwany. Serwer proxy na serwerze programu VMM i hosty funkcji Hyper-V należy zezwolić na dostęp do: <br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>Jeśli masz reguły zapory oparte na adresie IP na serwerze programu VMM, upewnij się, czy reguły zezwalają na komunikację z platformą Azure.<br/><br/> Zezwalaj na [zakresy IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) i HTTPS (port 443).<br/><br/>Zezwalaj na zakresy adresów IP dla regionu platformy Azure dla subskrypcji i zachodnie stany USA (używanych do zarządzania tożsamości i kontroli dostępu).<br/><br/> |

### <a name="replicated-machine-prerequisites"></a>Wymagania wstępne dotyczące replikowane maszyny

| **Składnik** | **Szczegóły** |
| --- | --- |
| **Chronione maszyny wirtualne** | Usługa Site Recovery obsługuje wszystkich systemów operacyjnych, które są obsługiwane przez [Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).<br/><br/>Maszyny wirtualne muszą spełniać [wymagania wstępne platformy Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) do tworzenia maszyny Wirtualnej platformy Azure. Nazwy komputerów musi wynosić od 1 do 63 znaków (należy użyć litery, cyfry i łączniki). Nazwa musi zaczynać się literą lub cyfrą i kończyć literą lub cyfrą. <br/><br/>Nazwa maszyny Wirtualnej można zmodyfikować po włączeniu replikacji dla maszyny Wirtualnej. <br/><br/> Pojemność dysku dla każdego chronionego komputera nie może być więcej niż 1,023 GB. Maszyna wirtualna może mieć maksymalnie 16 dysków (maksymalnie 16 TB).<br/><br/>


## <a name="disaster-recovery-of-hyper-v-vms-in-vmm-clouds-to-a-customer-owned-site"></a>Odzyskiwanie po awarii maszyn wirtualnych funkcji Hyper-V w chmurach VMM do lokacji należące do klientów

Następujące składniki są wymagane podczas odzyskiwania po awarii maszyn wirtualnych funkcji Hyper-V w chmurach VMM do lokacji należące do klientów. Są to oprócz firma opisana w [wymagania dotyczące usługi Azure](#azure-requirements).

| **Składnik** | **Szczegóły** |
| --- | --- |
| **Program Virtual Machine Manager** |  Zaleca się wdrożenie serwera programu VMM na lokacji głównej i dodatkowej lokacji.<br/><br/> Replikację między chmurami na jednym serwerze programu VMM, należy co najmniej dwóch chmur skonfigurowane na serwerze programu VMM.<br/><br/> Serwery programu VMM musi działać co najmniej System Center 2012 SP1 z najnowszymi aktualizacjami.<br/><br/> Każdy serwer VMM musi mieć co najmniej jedną chmurę. Wszystkie chmury musi mieć zestawu profilu pojemności funkcji Hyper-V. <br/><br/>Chmury muszą mieć co najmniej jedną grupę hostów programu VMM. Aby uzyskać więcej informacji na temat konfigurowania chmur programu VMM, zobacz [przygotowanie do wdrożenia usługi Azure Site Recovery](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric). |
| **Funkcja Hyper-V** | Serwery funkcji Hyper-V musi działać co najmniej Windows Server 2012 z rolą funkcji Hyper-V jest włączona i mieć zainstalowane najnowsze aktualizacje.<br/><br/> Serwer funkcji Hyper-V musi mieć przynajmniej jednej maszyny wirtualnej.<br/><br/>  Serwery hosta funkcji Hyper-V muszą znajdować się w grupach hostów w głównych i dodatkowych chmurach VMM.<br/><br/> Po uruchomieniu funkcji Hyper-V w klastrze systemu Windows Server 2012 R2, zalecamy, aby zainstalować aktualizację opisaną w artykule bazy wiedzy [2961977](https://support.microsoft.com/kb/2961977).<br/><br/> Jeśli uruchamianie funkcji Hyper-V w klastrze, w systemie Windows Server 2012 i statyczne klaster oparte na adresie IP, broker klastra nie jest tworzone automatycznie. Należy ręcznie skonfigurować brokera klastra. Aby uzyskać więcej informacji na temat brokera klastra, zobacz [skonfigurować dla replikacji do klastra rolę brokera repliki](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx). |
| **Dostawca** | Podczas wdrażania usługi Site Recovery Zainstaluj dostawcę usługi Azure Site Recovery na serwerach VMM. Dostawca komunikuje się z usługą Site Recovery za pośrednictwem protokołu HTTPS (port 443) w celu organizowania replikacji. Dane replikacji między serwerami funkcji Hyper-V podstawowych i pomocniczych za pomocą sieci LAN lub za pośrednictwem połączenia sieci VPN.<br/><br/> Dostawcy, który jest uruchomiony na serwerze VMM musi mieć dostęp do następujących adresów URL:<br/><br/>[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)] <br/><br/>Dostawcy usługi Site Recovery muszą zezwalać na komunikację zapory z serwerów programu VMM do [zakresy IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)i Zezwalaj na protokół HTTPS (port 443). |


## <a name="url-access"></a>Dostęp do adresu URL
Te adresy URL muszą być dostępne z Serwery hosta funkcji Hyper-V, VMware i VMM:

|**ADRES URL** | **Program VMM do programu VMM** | **Z programu VMM na platformę Azure** | **Z funkcji Hyper-V do platformy Azure** | **Z programu VMware do platformy Azure** |
|--- | --- | --- | --- | --- |
|``*.accesscontrol.windows.net`` | Zezwalaj | Zezwalaj | Zezwalaj | Zezwalaj |
|``*.backup.windowsazure.com`` | Niewymagane | Zezwalaj | Zezwalaj | Zezwalaj |
|``*.hypervrecoverymanager.windowsazure.com`` | Zezwalaj | Zezwalaj | Zezwalaj | Zezwalaj |
|``*.store.core.windows.net`` | Zezwalaj | Zezwalaj | Zezwalaj | Zezwalaj |
|``*.blob.core.windows.net`` | Niewymagane | Zezwalaj | Zezwalaj | Zezwalaj |
|``https://dev.mysql.com/get/archives/mysql-5.5/mysql-5.5.37-win32.msi`` | Niewymagane | Niewymagane | Niewymagane | Zezwalaj na pobieranie programu SQL |
|``time.windows.com`` | Zezwalaj | Zezwalaj | Zezwalaj | Zezwalaj|
|``time.nist.gov`` | Zezwalaj | Zezwalaj | Zezwalaj | Zezwalaj |
