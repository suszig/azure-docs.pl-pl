---
title: "Przygotowanie maszyny, aby skonfigurować odzyskiwania po awarii między regiony platformy Azure po zakończeniu migracji na platformie Azure przy użyciu usługi Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób przygotowania maszyny, aby skonfigurować odzyskiwania po awarii między regiony platformy Azure po zakończeniu migracji na platformie Azure przy użyciu usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: ponatara
ms.openlocfilehash: 559e64ae3c16ed21bc09ac0c044281aa0a6e43df
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2018
---
# <a name="replicate-azure-vms-to-another-region-after-migration-to-azure-by-using-azure-site-recovery"></a>Replikowanie maszyn wirtualnych Azure w innym regionie po migracji na platformie Azure przy użyciu usługi Azure Site Recovery

>[!NOTE]
> Azure Site Recovery replikacji maszyn wirtualnych platformy Azure (VM) jest obecnie w przeglądzie.

## <a name="overview"></a>Przegląd

Ten artykuł pomaga przygotować maszyn wirtualnych platformy Azure dla replikacji między dwóch regionach platformy Azure po tych maszyn zostały poddane migracji ze środowiska lokalnego do platformy Azure przy użyciu usługi Azure Site Recovery.

## <a name="disaster-recovery-and-compliance"></a>Odzyskiwanie po awarii i zgodność
Obecnie coraz więcej przedsiębiorstw przenosisz ich obciążeń na platformie Azure. Z przedsiębiorstw przenoszenie krytycznym lokalnymi obciążeń produkcyjnych Azure, konfigurowanie odzyskiwania po awarii dla tych zadań jest obowiązkowy, zgodności i ochronę przed jakichkolwiek potencjalnych zakłóceń w regionie platformy Azure.

## <a name="steps-for-preparing-migrated-machines-for-replication"></a>Kroki przygotowania zmigrowanych maszyn do replikacji
Aby przygotować zmigrowane maszyny do konfigurowania replikacji do innego regionu Azure:

1. Kończenie migracji.
2. Zainstaluj agenta platformy Azure, w razie potrzeby.
3. Usuń usługi mobilności.  
4. Uruchom ponownie maszynę wirtualną.

Te kroki są opisane bardziej szczegółowo w poniższych sekcjach.

### <a name="step-1-migrate-workloads-running-on-hyper-v-vms-vmware-vms-and-physical-servers-to-run-on-azure-vms"></a>Krok 1: Migracji obciążeń uruchomionych na maszynach wirtualnych funkcji Hyper-V, maszyn wirtualnych VMware i serwerów fizycznych do uruchamiania na maszynach wirtualnych platformy Azure

Konfigurowanie replikacji i przeprowadzić migrację lokalnej funkcji Hyper-V, VMware i obciążeń fizycznych do platformy Azure, postępuj zgodnie z instrukcjami [maszyn wirtualnych IaaS platformy Azure migracji między regiony platformy Azure z usługą Azure Site Recovery](site-recovery-migrate-azure-to-azure.md) artykułu. 

Po zakończeniu migracji nie trzeba przekazać lub usunąć awarię. Zamiast tego należy wybrać **przeprowadzenia migracji** opcji dla każdego komputera, które chcesz przeprowadzić migrację:
1. W obszarze **Replikowane elementy** kliknij prawym przyciskiem myszy maszynę wirtualną i kliknij pozycję **Zakończ migrację**. Kliknij przycisk **OK** do ukończenia tego kroku. Możesz śledzić postępy we właściwościach maszyny Wirtualnej, monitorując zadanie przeprowadzenia migracji w **zadania usługi Site Recovery**.
2. **Przeprowadzenia migracji** akcji kończy proces migracji, spowoduje usunięcie replikacji dla maszyny i zatrzymuje rozliczeń usługi Site Recovery dla maszyny.

### <a name="step-2-install-the-azure-vm-agent-on-the-virtual-machine"></a>Krok 2: Zainstaluj agenta maszyny Wirtualnej platformy Azure na maszynie wirtualnej
Azure [agenta maszyny Wirtualnej](../../virtual-machines/windows/agent-user-guide.md) musi być zainstalowany na maszynie wirtualnej w celu rozszerzenia usługi Site Recovery pracę i ułatwi ochronę maszyny Wirtualnej.

>[!IMPORTANT]
>Począwszy od wersji 9.7.0.0, w przypadku maszyn wirtualnych systemu Windows, Instalatora usługi mobilnej instaluje najnowsze dostępne Azure agenta maszyny Wirtualnej. Migracji maszyna wirtualna spełnia wymagań wstępnych dla przy użyciu dowolnego rozszerzenia maszyny Wirtualnej z rozszerzeniem usługi Site Recovery instalacji agenta. Agent maszyny Wirtualnej platformy Azure musi zostać zainstalowany ręcznie tylko wtedy, gdy usługa mobilności zainstalowane na zmigrowanej maszynie wersji 9.6 lub starszej.

Poniższa tabela zawiera dodatkowe informacje na temat instalowania agenta maszyny Wirtualnej i sprawdzania poprawności, która została zainstalowana:

| **Operacja** | **Windows** | **Linux** |
| --- | --- | --- |
| Instalowanie agenta maszyny Wirtualnej |Pobierz i zainstaluj [plik MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Potrzebne są uprawnienia administratora, aby zakończyć instalację. |Zainstaluj najnowszą [agenta systemu Linux](../../virtual-machines/linux/agent-user-guide.md). Potrzebne są uprawnienia administratora, aby zakończyć instalację. Zaleca się zainstalowanie agenta z repozytorium dystrybucji. Firma Microsoft *nie jest zalecane* Instalowanie agenta maszyny Wirtualnej systemu Linux bezpośrednio z usługi GitHub.  |
| Sprawdzanie poprawności instalacji agenta maszyny Wirtualnej |1. Przejdź do folderu C:\WindowsAzure\Packages w maszynie Wirtualnej platformy Azure. Plik WaAppAgent.exe powinna zostać wyświetlona. <br>2. Kliknij plik prawym przyciskiem myszy, przejdź do opcji **Właściwości**, a następnie wybierz kartę **Szczegóły**. **Wersji produktu** pole powinno być 2.6.1198.718 lub nowszej. |ND |


### <a name="step-3-remove-the-mobility-service-from-the-migrated-virtual-machine"></a>Krok 3: Usuń usługi mobilności z maszyny wirtualnej

W przypadku migracji z lokalnych maszyn VMware lub serwerów fizycznych w systemie Windows/Linux, należy ręcznie usunąć/odinstalować usługi mobilności z maszyny wirtualnej.

>[!IMPORTANT]
>Ten krok nie jest wymagane dla maszyn wirtualnych funkcji Hyper-V do platformy Azure.

#### <a name="uninstall-the-mobility-service-on-a-windows-server-vm"></a>Odinstaluj usługę mobilności na maszynie Wirtualnej systemu Windows Server
Użyj jednej z następujących metod można odinstalować usługi mobilności na komputerze z systemem Windows Server.

##### <a name="uninstall-by-using-the-windows-ui"></a>Odinstaluj przy użyciu interfejsu użytkownika systemu Windows
1. W Panelu sterowania, wybierz **programy**.
2. Wybierz **Microsoft Azure Site odzyskiwania mobilności usługi/główny serwer docelowy**, a następnie wybierz **Odinstaluj**.

##### <a name="uninstall-at-a-command-prompt"></a>Odinstaluj w wierszu polecenia
1. Otwórz okno wiersza polecenia z uprawnieniami administratora.
2. Aby odinstalować usługi mobilności, uruchom następujące polecenie:

   ```
   MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
   ```

#### <a name="uninstall-the-mobility-service-on-a-linux-computer"></a>Odinstaluj usługę mobilności na komputerze z systemem Linux
1. Na serwerze Linux, zaloguj się jako **głównego** użytkownika.
2. W terminalu przejdź do /user/local/ASR.
3. Aby odinstalować usługi mobilności, uruchom następujące polecenie:

   ```
   uninstall.sh -Y
   ```

### <a name="step-4-restart-the-vm"></a>Krok 4: Ponowne uruchomienie maszyny Wirtualnej

Po odinstalowaniu usługi mobilności, uruchom ponownie maszynę Wirtualną przed skonfigurowaniem replikacji do innego regionu Azure.


## <a name="next-steps"></a>Kolejne kroki
- Włączyć ochronę obciążeń przez [replikowanie maszyn wirtualnych platformy Azure](azure-to-azure-quickstart.md).
- Dowiedz się więcej o [wskazówki dotyczące replikowanie maszyn wirtualnych platformy Azure networking](site-recovery-azure-to-azure-networking-guidance.md).
