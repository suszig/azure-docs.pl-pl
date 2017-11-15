---
title: "Ochrona serwera plików za pomocą usługi Azure Site Recovery"
description: "W tym artykule opisano sposób chronić serwer plików przy użyciu usługi Azure Site Recovery"
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/23/2017
ms.author: rajanaki
ms.custom: mvc
ms.openlocfilehash: cc585d6add9b9c5ce7f3379aeaf5ec79f5c61d51
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2017
---
# <a name="protect-a-file-server-by-using-azure-site-recovery"></a>Ochrona serwera plików za pomocą usługi Azure Site Recovery 

[Usługi Azure Site Recovery](site-recovery-overview.md) usługi przyczynia się do ciągłości i odzyskiwaniem po awarii (BCDR) odzyskiwania strategią biznesową przy zachowaniu aplikacji biznesowych dostępne podczas planowanych lub nieplanowanych przestojów. Usługa Site Recovery zarządza i organizuje odzyskiwania po awarii maszyn lokalnych i maszyn wirtualnych platformy Azure (maszyny wirtualne), łącznie z replikacji, trybu failover i odzyskiwania różnych obciążeń.

W tym artykule opisano, jak chronić serwer plików przy użyciu usługi Azure Site Recovery i inne zalecenia do potrzeb różnych środowiskach.

## <a name="file-server-architecture"></a>Architektura serwera plików
Otwarte, rozproszonej udostępnianie plików systemu udostępnia środowisko gdzie geograficznie rozproszonych użytkownicy mogą współpracować z plikami bez naruszania wymagania integralności. 

Typowy lokalnymi ekosystemu serwera plików obsługuje dużej liczby równoczesnych użytkowników i elementy zawartości używa rozproszonych replikacji systemu plików (DFSR) dla replikacji planowania i ograniczania przepustowości. DFSR używa algorytmu kompresji zdalnego kompresji RDC (RDC), który może służyć do wydajnie pliki aktualizacji w ograniczonej przepustowości sieci. Algorytm RDC wykrywa wstawienia, usuwania i zmianę położenia danych w plikach. Umożliwia on DFSR replikowanie tylko zmienionych bloków pliku podczas aktualizacji plików. 

W niektórych środowiskach serwera plików codzienne kopie zapasowe są pobierane w czasie poza szczytem odzyskiwania po awarii. Nie należy używać tych środowisk DFSR.

Następujące topologii przedstawiono środowisku serwerów plików z DFSR zaimplementowana:
                
![Architektura usługi DFSR](media/site-recovery-file-server/dfsr-architecture.JPG)

Na rysunku wielu serwerów plików (określanych jako elementy członkowskie) aktywnie uczestniczyć w replikacji plików między grupą replikacji. Zawartość w folderze replikowanym są dostępne dla wszystkich klientów, którzy wysyłają żądania do jednego z elementów członkowskich, nawet wtedy, gdy jeden z członków przejdzie do trybu offline.

## <a name="disaster-recovery-strategies-for-file-servers"></a>Strategii odzyskiwania danych na serwerach plików

- **Replikowanie serwera plików na platformę Azure przy użyciu usługi Azure Site Recovery**: gdy jeden lub więcej serwerów plików lokalnych są niedostępne, można włączane odzyskiwanie maszyn wirtualnych na platformie Azure. Odzyskiwanie maszyn wirtualnych można następnie obsługiwać żądań od klientów, w infrastrukturze lokalnej, jeśli istnieje połączenie sieci VPN typu lokacja lokacja i usługi Active Directory jest skonfigurowany w usłudze Azure. Można to zrobić w środowisku skonfigurowane DFSR lub w środowisku serwera prostego pliku z nie DFSR. 

- **Rozszerzenie do maszyny Wirtualnej platformy Azure IaaS DFSR**: W środowisku serwera plików z DFSR zaimplementowana jedną zalecane podejście jest rozszerzenie DFSR lokalnego do platformy Azure. Maszyna wirtualna platformy Azure można wykonywać rolę serwera plików. 

    Po zależności połączenie sieci VPN typu lokacja lokacja i usługi Active Directory są obsługiwane i DFSR znajduje się w miejscu, gdy jeden lub więcej serwerów plików lokalnych są niedostępne, klienci nadal mogą połączenie z maszyną Wirtualną Azure. Maszyna wirtualna Azure obsługują żądania.

    Zalecamy tej metody, jeśli maszyny wirtualne mają konfiguracje, które nie obsługują usługi Azure Site Recovery. Przykładem takiej konfiguracji to dysk udostępniony klaster, który jest powszechnie używany w środowisku serwerów plików. DFSR również działa dobrze w środowiskach niskiej przepustowości przy użyciu zmian średnia. Z tej metody należy uwzględnić koszty dodatkowe o maszynie Wirtualnej platformy Azure w i uruchomione przez cały czas.  

- **Użyj synchronizacji plików Azure w celu zreplikowania plików**: Jeśli przygotowywania dla podróży do chmury lub już z maszyny Wirtualnej platformy Azure, zalecamy użycie synchronizacji plików Azure. Ta usługa udostępnia synchronizowanie udziałów plików pełni zarządzane w chmurze, które są dostępne za pośrednictwem standardu branżowego [bloku komunikatów serwera ](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx)protokołu (SMB). Następnie można udziałów plików Azure instalacji jednocześnie w chmurze lub lokalnie wdrożeń systemu Windows, Linux i macOS. 

Poniższy diagram ułatwia zdecyduj, jakie strategii dla danego środowiska serwera plików:

![drzewa decyzyjnego](media/site-recovery-file-server/decisiontree.png)


### <a name="factors-for-making-a-disaster-recovery-decision"></a>Czynniki podjęciem decyzji o odzyskiwania po awarii

|Środowisko  |Zalecenie  |Kwestie do rozważenia |
|---------|---------|---------|
|Środowisko serwera plików lub bez DFSR|   [Użyć usługi Azure Site Recovery na potrzeby replikacji](#replicate-an-on-premises-file-server-by-using-azure-site-recovery)   |    Usługa Site Recovery nie obsługuje udostępnione klastry dysków lub NAS. Jeśli środowisko używa jednej z tych opcji, należy użyć jednej z innych metod zależnie od potrzeb. <br> Usługa Azure Site Recovery nie obsługuje protokołu SMB 3.0. Zreplikowanej maszyny Wirtualnej będzie zastosować zmiany wprowadzone do plików tylko wtedy, gdy zmiany są uwzględniane w oryginalnej lokalizacji pliku. 
|Środowisko serwera plików z DFSR     |  [Rozszerzenie DFSR maszyny wirtualnej platformy Azure IaaS](#extend-dfsr-to-an-azure-iaas-virtual-machine)  |      DFSR działa dobrze w środowiskach crunched przepustowości. Takie podejście, jednak wymaga posiadania maszyny Wirtualnej platformy Azure i uruchomić cały czas. Planowaniu wymaga konta kosztów maszyny Wirtualnej.         |
|Maszyna wirtualna platformy Azure IaaS     |     [Synchronizację plików na platformę Azure](#use-azure-file-sync-to-replicate-your-on-premises-files)   |     W przypadku odzyskiwania po awarii Jeśli używasz synchronizacji plików Azure w trybie failover, należy wykonać działania ręczne, aby upewnić się, że udziały plików są dostępne w sposób przezroczysty do komputera klienckiego. Synchronizacja programu Azure pliku wymaga port 445 być otwarty z komputera klienta.     |


### <a name="site-recovery-support"></a>Obsługa odzyskiwania lokacji
Ponieważ replikacja usługi Site Recovery jest niezależny od aplikacji, zalecenia zawarte w tym miejscu powinny dotyczy w następujących scenariuszach:
| Element źródłowy    |Do lokacji dodatkowej    |Na platformie Azure
|---------|---------|---------|
|Azure| -|Tak|
|Funkcja Hyper-V|   Tak |Tak
|VMware |Tak|   Tak
|Serwer fizyczny|   Tak |Tak
 

> [!IMPORTANT]
> Przed przystąpieniem do jednej z następujących metod, pamiętaj, aby adres zależności.

**Połączenie lokacja-lokacja**: można nawiązać bezpośrednie połączenie między lokacją lokalną i sieć platformy Azure Zezwalaj na komunikację między serwerami. Można używać bezpiecznego połączenia sieci VPN lokacja lokacja do sieci wirtualnej platformy Azure, która będzie służyć jako lokacja odzyskiwania po awarii. Aby uzyskać więcej informacji, zobacz [utworzyć połączenie lokacja-lokacja w portalu Azure](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal).

**Usługi Active Directory**: DFSR zależy od usługi Active Directory. Oznacza to, że lasu usługi Active Directory za pomocą lokalnych kontrolerów domeny jest rozszerzony do lokacji odzyskiwania po awarii na platformie Azure. Nawet jeśli nie używasz DFSR, jeśli użytkownicy muszą być przyznane/zweryfikować dostępu, należy wykonać następujące kroki.
Aby uzyskać więcej informacji, zobacz [Rozszerz lokalnej usługi Active Directory na platformie Azure](https://docs.microsoft.com/azure/site-recovery/site-recovery-active-directory).

## <a name="disaster-recovery-recommendations-for-azure-iaas-virtual-machines"></a>Zalecenia dotyczące odzyskiwania po awarii dla maszyn wirtualnych IaaS platformy Azure

Jeśli są konfigurowanie i zarządzanie odzyskiwaniem hostowanych na maszynach wirtualnych Azure IaaS serwerów plików, możesz wybrać jedną z dwóch opcji: synchronizacja plików Azure i usługi Azure Site Recovery. Decyzja zależy od tego, czy chcesz przenieść [plików Azure](https://docs.microsoft.com/azure/storage/files/storage-files-introduction).

### <a name="use-azure-file-sync-to-replicate-files-hosted-on-iaas-virtual-machines"></a>Synchronizację plików Azure replikację plików hostowanych na maszynach wirtualnych IaaS

Pliki Azure umożliwia całkowicie zastępujące lub uzupełniające, serwerów plików tradycyjnych, lokalnie lub urządzeniach NAS. Udziały plików platformy Azure, również mogą być replikowane z synchronizacji plików Azure na serwerach z systemem Windows, lokalnie lub w chmurze, wydajność oraz systemy buforowania rozproszonego danych, w którym jest używany. 

Poniższe kroki szczegółowo zalecenie dotyczące odzyskiwania po awarii dla maszyn wirtualnych platformy Azure, który wykonać taką samą funkcję jak serwery plików tradycyjny:
1. Ochrona maszyn za pomocą usługi Azure Site Recovery za pośrednictwem kroki wymienione w tym miejscu.

2. Synchronizacja plików Azure umożliwia replikację plików z maszyny Wirtualnej, który działa jako serwer plików do chmury.

3. Użyj [planu odzyskiwania](https://docs.microsoft.com/en-us/azure/site-recovery/site-recovery-create-recovery-plans) funkcji w usłudze Azure Site Recovery do dodawania skryptów do [instalowanie udziału plików na platformę Azure](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) i uzyskania dostępu do udziału na maszynie wirtualnej.

W poniższych krokach opisano krótko jak używać usługi synchronizacji plików Azure:

1. [Utwórz konto magazynu na platformie Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). W przypadku wybrania magazynu geograficznie nadmiarowego dostęp do odczytu dla kont magazynu w regionie pomocniczym w przypadku awarii zostanie wyświetlony dostęp do odczytu do danych. Aby uzyskać więcej informacji, zobacz [strategii odzyskiwania danych udział plików Azure](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

2. [Tworzenie udziału plików](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).

3. [Wdrażanie synchronizacji plików Azure](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) na serwerze plików na platformę Azure.

4. Utwórz grupę synchronizacji. Punkty końcowe w ramach grupy synchronizacji zostanie pozostają zsynchronizowane ze sobą. Grupy synchronizacji musi zawierać co najmniej jedna chmura punktu końcowego, który reprezentuje udział plików Azure, i co serwer punktu końcowego, który reprezentuje ścieżkę, w systemie Windows server.  
    Pliki będą teraz pozostają zsynchronizowane między udziału plików platformy Azure i na serwerze lokalnym.

5. Jeśli istnieje awarii w środowisku lokalnym, należy wykonać trybu failover przy użyciu planu odzyskiwania. Dodaj skrypt [instalowanie udziału plików na platformę Azure](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) i uzyskania dostępu do udziału na maszynie wirtualnej.

### <a name="replicate-an-iaas-file-server-virtual-machine-by-using-azure-site-recovery"></a>Replikuj maszyny wirtualnej serwera IaaS pliku za pomocą usługi Azure Site Recovery

Jeśli masz lokalnej klientom dostęp do maszyny wirtualnej serwera plików IaaS, wykonaj następujące kroki. W przeciwnym razie wykonaj krok 3.

1. Nawiąż połączenie sieci VPN typu lokacja lokacja między lokacją lokalną i sieć platformy Azure.  

2. Rozszerzenie w lokalnej usłudze Active Directory.

3. [Konfigurowanie odzyskiwania po awarii](azure-to-azure-tutorial-enable-replication.md) IaaS maszyny serwera plików w regionie pomocniczym.

Aby uzyskać więcej informacji dotyczących odzyskiwania po awarii w regionie pomocniczym, zobacz [architektura replikacji Azure do platformy Azure](concepts-azure-to-azure-architecture.md).

## <a name="disaster-recovery-recommendations-for-on-premises-virtual-machines"></a>Zalecenia dotyczące odzyskiwania po awarii dla lokalnych maszyn wirtualnych 

### <a name="replicate-an-on-premises-file-server-by-using-azure-site-recovery"></a>Replikowanie lokalnych serwera plików za pomocą usługi Azure Site Recovery
Poniższe kroki szczegółowo replikacji dla maszyny Wirtualnej VMware. Kroki do replikowania maszyny Wirtualnej funkcji Hyper-V, zobacz [Konfigurowanie odzyskiwania po awarii maszyn wirtualnych funkcji Hyper-V lokalnego do platformy Azure](https://docs.microsoft.com/en-us/azure/site-recovery/tutorial-hyper-v-to-azure).

1. [Przygotowanie zasobów Azure](tutorial-prepare-azure.md) dla replikację maszyn lokalnych.

2. Nawiąż połączenie sieci VPN typu lokacja lokacja między lokacją lokalną i sieć platformy Azure.  

3. Rozszerzenie w lokalnej usłudze Active Directory.

4. [Przygotowanie serwerów VMware lokalnych](tutorial-prepare-on-premises-vmware.md).

5. [Konfigurowanie odzyskiwania po awarii](tutorial-vmware-to-azure.md) Azure dla maszyn wirtualnych lokalnie.

### <a name="extend-dfsr-to-an-azure-iaas-virtual-machine"></a>Rozszerzenie DFSR maszyny wirtualnej platformy Azure IaaS

1. Nawiąż połączenie sieci VPN typu lokacja lokacja między lokacją lokalną i sieć platformy Azure. 

2. Rozszerzenie w lokalnej usłudze Active Directory.

3. [Tworzenie i Inicjowanie ich obsługi serwera plików maszyny Wirtualnej](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) w sieci wirtualnej platformy Azure.  
    Upewnij się, że maszyna wirtualna jest dodana do tej samej sieci wirtualnej platformy Azure, z łącznością między ze środowiska lokalnego. 

4. Zainstaluj i [Konfigurowanie replikacji systemu plików DFS](https://blogs.technet.microsoft.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx) w systemie Windows Server.

5. [Implementowanie przestrzeni nazw systemu plików DFS](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/deploying-dfs-namespaces).

6. Z przestrzeni nazw systemu plików DFS, zaimplementowana można przełączyć folderów udostępnionych z produkcji lokacjach odzyskiwania po awarii, aktualizując obiektów docelowych folderów przestrzeni nazw systemu plików DFS. Po tych systemu plików DFS replikacji zmian przestrzeni nazw przy użyciu usługi Active Directory, użytkownicy są podłączone do odpowiedniego folderu obiektów docelowych przezroczysty.

### <a name="use-azure-file-sync-to-replicate-your-on-premises-files"></a>Użyj synchronizacji plików Azure w celu zreplikowania plików lokalnych
Za pomocą usługi Azure synchronizacji plików, pliki, które można replikować do chmury. W przypadku awarii i niedostępność serwera plików lokalnej możesz zainstalować lokalizacje żądanego pliku z chmury i kontynuować do obsługi żądań od komputerów klienckich.

Sugerowane rozwiązanie integracji Azure plik synchronizacji z usługą Azure Site Recovery jest:
1. Ochrona maszyny serwera plików za pomocą usługi Azure Site Recovery. Postępuj zgodnie z instrukcjami [ustawienia odzyskiwania po awarii do platformy Azure dla maszyn wirtualnych VMware lokalnymi](tutorial-vmware-to-azure.md).

2. Synchronizacja plików Azure umożliwia replikację plików z komputera, który służy jako serwer plików do chmury.

3. Użyj funkcji planu odzyskiwania w usłudze Azure Site Recovery, aby dodać skryptów, aby zainstalować udział plików na platformę Azure na serwerze plików przełączona w tryb failover maszyny Wirtualnej na platformie Azure.

Następujące szczegółowe kroki przy użyciu usługi Azure synchronizacji plików:

1. [Utwórz konto magazynu na platformie Azure](https://docs.microsoft.com/en-us/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Jeśli dostęp do odczytu magazynu geograficznie nadmiarowego (zalecane) w przypadku kont magazynu, konieczne będzie dostęp do odczytu danych z regionu pomocniczego w przypadku awarii. Aby uzyskać więcej informacji, zobacz [strategii odzyskiwania danych udział plików Azure](https://docs.microsoft.com/en-us/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

2. [Tworzenie udziału plików](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share).

3. [Wdrażanie synchronizacji plików Azure](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) w lokalnej serwera plików.

4. Utwórz grupę synchronizacji. Punkty końcowe w ramach grupy synchronizacji zostanie pozostają zsynchronizowane ze sobą. Grupy synchronizacji musi zawierać co najmniej jedna chmura punktu końcowego, który reprezentuje udział plików na platformę Azure, i jeden serwer punktu końcowego, który reprezentuje ścieżkę, w systemie Windows server lokalnymi.  
    Pliki będą teraz pozostają zsynchronizowane między udziału plików platformy Azure i na serwerze lokalnym.

5. Jeśli istnieje awarii w środowisku lokalnym, należy wykonać trybu failover przy użyciu planu odzyskiwania. Dodawanie skryptu, aby zainstalować udział plików na platformę Azure i uzyskać dostępu do udziału na maszynie wirtualnej.

> [!NOTE]
> Upewnij się, że jest otwarty port 445. Usługa pliki Azure korzysta z protokołu SMB. SMB komunikuje się za pośrednictwem portu TCP 445. Sprawdź, czy Zapora blokuje port 445 z komputera klienta.


## <a name="perform-a-test-failover"></a>Wykonaj test trybu failover

1. Przejdź do portalu Azure i wybierz magazyn usług odzyskiwania.

2. Wybierz plan odzyskiwania utworzone w środowisku serwera plików.

3. Wybierz **testowanie trybu Failover**.

4. Wybierz punkt odzyskiwania i sieć wirtualna platformy Azure, aby rozpocząć proces test trybu failover.

5. Po skonfigurowaniu dodatkowej środowiska wykonania programu operacji sprawdzania poprawności.

6. Po zakończeniu operacji sprawdzania poprawności można wybrać **oczyszczania testowy tryb failover** w planie odzyskiwania i testowania trybu failover jest czyszczona środowiska.

Aby uzyskać więcej informacji o wykonywaniu test trybu failover, zobacz [testowanie trybu failover na platformie Azure w usłudze Site Recovery](site-recovery-test-failover-to-azure.md).

Aby uzyskać wskazówki dotyczące przeprowadzania testu pracy awaryjnej dla usługi Active Directory i DNS, zobacz [Test pracy awaryjnej zagadnienia dotyczące usługi Active Directory i DNS](site-recovery-active-directory.md).

## <a name="perform-a-failover"></a>Przejściu w tryb failover

1. Przejdź do portalu Azure i wybierz magazyn usług odzyskiwania.

2. Wybierz plan odzyskiwania utworzone w środowisku serwera plików.

3. Wybierz **pracy awaryjnej**.

4. Wybierz punkt odzyskiwania, aby rozpocząć proces trybu failover.

Aby uzyskać więcej informacji na temat wykonywania pracy awaryjnej, zobacz [pracy awaryjnej w usłudze Site Recovery](site-recovery-failover.md).
