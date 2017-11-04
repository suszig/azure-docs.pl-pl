---
title: "Uaktualnienie w magazynie usługi Site Recovery w magazynie usług odzyskiwania Azure"
description: "Dowiedz się, jak uaktualnić magazynie usługi Azure Site Recovery do magazynu usług odzyskiwania"
documentationcenter: 
author: rajani-janaki-ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/31/2017
ms.author: rajani-janaki-ram
ms.openlocfilehash: fdb33ea0d08353b491f2934fcf885fcb6910b9a2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="upgrade-a-site-recovery-vault-to-an-azure-resource-manager-based-recovery-services-vault"></a>Uaktualnij do magazynu usług odzyskiwania Azure Resource Manager w magazynie usługi Site Recovery

W tym artykule opisano sposób uaktualniania magazynów Azure Site Recovery do magazynów usług odzyskiwania Azure Resource Manager, bez wpływu na trwającej replikacji. Aby uzyskać więcej informacji na temat usługi Azure Resource Manager funkcje i korzyści, zobacz [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="introduction"></a>Wprowadzenie
Magazyn usług odzyskiwania jest zasobem usługi Azure Resource Manager do zarządzania kopii zapasowej i odzyskiwanie po awarii w chmurze. Jest on ujednoliconego magazyn, którego można używać w nowego portalu Azure, i zastępuje klasycznego kopii zapasowej i magazynów usługi Site Recovery.

Magazyny usług odzyskiwania oferują tablicę funkcje, takie jak:

* Pomoc techniczna platformy Azure Resource Manager: można chronić i pracy awaryjnej z maszyn wirtualnych i maszyn fizycznych do stosu usługi Azure Resource Manager.

* Wykluczanie dysku: Jeśli masz pliki tymczasowe lub wiele zmian danych, których nie chcesz korzystać z przepustowości dla woluminów można wykluczyć z replikacji. Ta funkcja została włączona w obecnie *VMware do platformy Azure* i *funkcji Hyper-V w systemie Azure* i jest rozszerzona na inne scenariusze, a także.

* Obsługa premium i Magazyn lokalnie nadmiarowy: mogą teraz chronić serwery w magazynie premium konta, które umożliwiają klientom wyższy ochronę aplikacji za pomocą operacji wejścia/wyjścia na sekundę (IOPS). Ta funkcja jest obecnie włączona w *VMware do platformy Azure*.

* Uproszczone środowisko — wprowadzenie: udoskonalone środowisko — wprowadzenie został zaprojektowany, aby ułatwić Instalator odzyskiwania po awarii.

* Kopii zapasowych i odzyskiwania lokacji zarządzania z tego samego magazynu: można teraz chronić serwerów na potrzeby odzyskiwania po awarii lub wykonaj kopię zapasową z tego samego magazynu, co może zmniejszyć nakłady znacznie z zarządzania.

Aby uzyskać więcej informacji na temat funkcji i doświadczeń uaktualniony, zobacz [blogu magazynu kopii zapasowej i odzyskiwania](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).

## <a name="salient-features"></a>Najważniejsze funkcje

* Bez wpływu na trwająca replikacja: trwającej replikacji kontynuować bez przerwy podczas i po uaktualnieniu.

* Bez dodatkowych kosztów: Pobierz całego zestawu funkcji zaktualizowane bez ponoszenia dodatkowych kosztów.

* Brak utraty danych: ponieważ ten proces jest uaktualnienie i nie migracji, istniejących punktów odzyskiwania replikacji i ustawienia pozostaną nienaruszone podczas i po uaktualnieniu.


## <a name="what-happens-during-the-vault-upgrade"></a>Co się dzieje podczas uaktualniania magazynu?

Podczas uaktualniania nie można wykonać operacji, takich jak rejestrowanie nowego serwera oraz włączania replikacji dla maszyny wirtualnej (VM). Operacje, które wymagają Odczyt danych z lub zapisywania danych do magazynu, takich jak trwająca replikacja chronionych elementów w magazynie nadal przerwana.

### <a name="changes-to-automation-and-tooling-after-the-upgrade"></a>Zmiany do automatyzacji i narzędzi po uaktualnieniu
Po uaktualnieniu typu magazynu w klasycznym modelu wdrażania modelu wdrażania usługi Resource Manager, należy zaktualizować istniejącej automatyzacji lub narzędzi, aby upewnić się, że nadal działać po uaktualnieniu.

### <a name="prepare-your-environment-for-the-upgrade"></a>Przygotowywanie środowiska dla uaktualnienia

* [Instalowanie programu PowerShell lub uaktualnić ją do wersji 5 lub nowszej](https://www.microsoft.com/download/details.aspx?id=50395)
* [Zainstaluj najnowszą wersję programu Azure PowerShell MSI](https://github.com/Azure/azure-powershell/releases)
* [Pobieranie skryptu uaktualniania magazynu usług odzyskiwania](https://aka.ms/vaultupgradescript)

### <a name="prerequisites"></a>Wymagania wstępne
Aby uaktualnić magazynów usługi Site Recovery do magazynów usług odzyskiwania Azure Resource Manager, muszą spełniać następujące wymagania:

* Wersja minimalna agenta: wersja zainstalowana na serwerze dostawcy usługi Azure Site Recovery musi być 5.1.1700.0 lub nowszym.

* Obsługiwana konfiguracja: nie można skonfigurować Magazyn sieci magazynowania (SAN) lub grup dostępności AlwaysOn programu SQL Server. Inne konfiguracje są obsługiwane.

    >[!NOTE]
    >Po uaktualnieniu można zarządzać mapowania przechowywania tylko przy użyciu programu PowerShell.

* Scenariusz wdrażania obsługiwanych: Magazyn nie powinien być *VMware do platformy Azure* modelu wdrażania starszej wersji. Przed kontynuowaniem, najpierw przenieść do modelu wdrożenia rozszerzonego.

* Operacje płaszczyzny żadnych aktywnych działań zainicjowane przez użytkownika, które obejmują zarządzanie: ponieważ podczas uaktualniania jest ograniczony do płaszczyzny zarządzania, wykonanie wszystkich akcji płaszczyzny zarządzania przed wyzwoleniem uaktualnienia. Ten proces nie zawiera trwającej replikacji.

## <a name="frequently-asked-questions"></a>Często zadawane pytania

**To uaktualnienie wpływa na mój trwającej replikacji?**

Nie. Twoje bieżące replikacja jest kontynuowana przerwana podczas i po uaktualnieniu.

**Co się dzieje z ustawień sieciowych, takich jak ustawienia IP i sieci VPN typu lokacja lokacja?**

Uaktualnienie nie ma wpływu na ustawienia sieciowe. Wszystkie połączenia Azure do lokalne pozostaną nienaruszone.

**Co się dzieje z Moje magazynów, jeśli nie było planowane w najbliższej przyszłości uaktualnienia?**

Obsługa magazynu usługi Site Recovery w portalu Azure starego zostaną wycofane uruchamianie września 2017 r. Zdecydowanie zaleca się użycie funkcji uaktualnienia można przenieść do nowego portalu.

**Co oznacza ten plan migracji dla moich istniejących narzędzi?**  

Aktualizacja z narzędziami do modelu wdrażania Menedżera zasobów jest jednym z najważniejszych zmian, które należy uwzględnić w planów uaktualnienia. Magazyny usług odzyskiwania są oparte na modelu wdrażania usługi Resource Manager. 

**Jak długo przestoju płaszczyzny zarządzania ostatniego?**

Uaktualnienie zwykle trwa około 15 do 30 minut, a może potrwać maksymalnie jedną godzinę.

**I przywrócić po uaktualnieniu?**

Nie. ROLLBACK nie jest obsługiwana, gdy zasoby zostały pomyślnie uaktualnione.

**Moja subskrypcja lub zasobów, aby sprawdzić, czy może zostać uaktualniony I zweryfikować?**

Tak. Obsługiwane platformy opcji uaktualniania pierwszym krokiem podczas uaktualnienia jest zweryfikować, że zasoby są w stanie uaktualnienia. W przypadku niepowodzenia weryfikacji, otrzymasz odpowiednie komunikaty o błędach i ostrzeżenia.

**Jak zgłosić problem po uaktualnieniu?**

Jeśli wystąpią zakończą się niepowodzeniem podczas uaktualniania, należy pamiętać, identyfikator operacji, który znajduje się w dzienniku błędów. Microsoft Support aktywnego będą działać w rozwiązaniu problemu. Można również skontaktować się z obsługą z Identyfikatorem subskrypcji, nazwę magazynu, a identyfikator operacji. Obsługa będzie działać, aby rozwiązać ten problem, tak szybko jak to możliwe. Nie próbuj ponownie wykonać operację, chyba że jawnie należy to zrobić przez firmę Microsoft.

## <a name="run-the-script"></a>Uruchom skrypt

W programie PowerShell uruchom następujące polecenie:

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionID <subscriptionID>  -VaultName <vaultname> -Location <location> -ResourceType HyperVRecoveryManagerVault -TargetResourceGroupName <rgname>

* Identyfikator subskrypcji: Identyfikator subskrypcji skojarzoną z magazynu, który w przypadku uaktualniania.

* VaultName: Nazwa magazynu, który w przypadku uaktualniania.

* Lokalizacja: Lokalizacja magazynu, który w przypadku uaktualniania.

* Typ zasobu: Magazyny HyperVRecoveryManagerVault Site Recovery.

* TargetResourceGroupName: Grupy zasobów do której ma zostać uaktualniona magazynu do umieszczenia. TargetResourceGroupName może być istniejącej grupy zasobów w usłudze Azure Resource Manager lub nowy. Jeśli TargetResourceGroupName, która jest dostarczana nie istnieje, jest tworzony w ramach uaktualnienia w tej samej lokalizacji co magazyn. Aby uzyskać więcej informacji, zobacz sekcję "Grupy zasobów" [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).

    >[!NOTE]
    >Nazwy grupy zasobów podlega pewnym ograniczeniom. Aby uniknąć niepowodzenia uaktualnienia magazynu, należy dokładnie sprawdzić konwencji nazewnictwa.
    >
    >Na przykład:
    >
    >.\RecoveryServicesVaultUpgrade-1.0.0.ps1 SubscriptionId - 1234-54123-354354-56416-8645 - VaultName gen2dr — lokalizacja "Europa Północna" hypervrecoverymanagervault - ResourceType - TargetResourceGroupName abc

Możesz też uruchomić poniższy skrypt. Wprowadź wartości dla wymaganych parametrów.

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1
    cmdlet RecoveryServicesVaultUpgrade-1.0.0.ps1 at command pipeline position 1

    Supply values for the following parameters:
    SubscriptionId:
    VaultName:
    Location:
    ResourceType:
    TargetResourceGroupName:

1. Skrypt programu PowerShell wyświetli monit o podanie poświadczeń. Wprowadź je dwukrotnie raz dla konta modelu wdrażania klasycznego i raz dla konta usługi Azure Resource Manager.

2. Po wprowadzeniu poświadczeń, skrypt zostanie uruchomiony sprawdzenie w celu określenia, czy ustawienia infrastruktury spełnia wymagania opisane powyżej.

3. Po wymagania wstępne zostały sprawdzone i potwierdzone, zostanie wyświetlony monit Przystąp do uaktualnienia magazynu. Rozpoczyna się proces uaktualniania, uaktualnianie magazynu. Cały uaktualnienie może potrwać od 15 do 30 minut.

4. Po Uaktualnianie zostało pomyślnie zakończone, można uzyskać dostęp do magazynu uaktualniony w portalu Azure.

## <a name="post-upgrade-vault-management"></a>Zarządzanie magazynem po uaktualnieniu

### <a name="replicate-by-using-azure-site-recovery-in-the-recovery-services-vault"></a>Replikowane za pomocą usługi Azure Site Recovery w magazynie usług odzyskiwania

* Teraz można chronić maszyny wirtualne Azure z jednego regionu do innego. Aby uzyskać więcej informacji, zobacz [replikowanie maszyn wirtualnych platformy Azure między regionami z usługą Azure Site Recovery](site-recovery-azure-to-azure.md).

* Aby uzyskać więcej informacji na temat replikację maszyn wirtualnych VMware do platformy Azure, zobacz [replikowanie maszyn wirtualnych VMware do platformy Azure z usługą Site Recovery](vmware-walkthrough-overview.md).

* Aby uzyskać więcej informacji na temat replikowania maszyn wirtualnych funkcji Hyper-V (bez VMM) na platformie Azure, zobacz [maszyn wirtualnych replikacji funkcji Hyper-V (bez VMM) na platformie Azure](hyper-v-site-walkthrough-overview.md).

* Aby uzyskać więcej informacji na temat replikowania maszyn wirtualnych funkcji Hyper-V (w programie VMM) na platformie Azure, zobacz [maszyn wirtualnych replikacji funkcji Hyper-V w chmurach VMM do platformy Azure przy użyciu usługi Site Recovery w portalu Azure](vmm-to-azure-walkthrough-overview.md).

* Aby uzyskać więcej informacji o replikacji funkcji Hyper-VMs (w programie VMM) do lokacji dodatkowej, zobacz [maszyn wirtualnych replikacji funkcji Hyper-V w chmurach VMM do dodatkowej lokacji programu VMM przy użyciu portalu Azure](site-recovery-vmm-to-vmm.md).

* Aby uzyskać więcej informacji na temat replikowania maszyn wirtualnych VMware do lokacji dodatkowej, zobacz [Replikowanie lokalnych maszyn wirtualnych VMware lub serwerów fizycznych do lokacji dodatkowej w klasycznym portalu Azure](site-recovery-vmware-to-vmware.md).

### <a name="view-your-replicated-items"></a>Wyświetl elementy replikowane

Na poniższej ilustracji przedstawiono strony pulpitu nawigacyjnego magazynu usług odzyskiwania, wyświetlająca jednostek klucza magazynu. Zaznacz, aby wyświetlić listę chronionych jednostki w magazynie **usługi Site Recovery** > **elementy replikowane**.


![Zreplikowanych elementów](./media/upgrade-site-recovery-vaults/replicateditems.png)

Na poniższej ilustracji przedstawiono przepływ pracy do wyświetlania elementów replikowanych i **pracy awaryjnej** polecenia inicjowania pracy awaryjnej.

![Zreplikowanych elementów](./media/upgrade-site-recovery-vaults/failover.png)

### <a name="view-your-replication-settings"></a>Wyświetl ustawienia replikacji

W magazynie usługi Site Recovery każdej grupy ochrony jest konfigurowana częstotliwość kopiowania, przechowywania punktu odzyskiwania, częstotliwość migawek spójności aplikacji i innych ustawień replikacji. W magazynie usług odzyskiwania te ustawienia są skonfigurowane jako zasady replikacji. Nazwa zasady jest nazwa grupy ochrony lub *primarycloud_Policy*.

Aby uzyskać więcej informacji na temat zasad replikacji, zobacz [Zarządzanie zasadami replikacji dla VMware do platformy Azure](site-recovery-setup-replication-settings-vmware.md).
