---
title: "Ochrona serwera plików przy użyciu usługi Azure Site Recovery"
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
ms.openlocfilehash: a746ace47c4f1190b7a695014543670a1a9cf879
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="protect-a-file-server-using-azure-site-recovery"></a>Ochrona serwera plików przy użyciu usługi Azure Site Recovery 

[Usługi Azure Site Recovery](site-recovery-overview.md) usługi przyczynia się do ciągłości i odzyskiwaniem po awarii (BCDR) odzyskiwania strategią biznesową, zatrzymując aplikacji biznesowych i uruchamiając dostępne podczas planowanych lub nieplanowanych przestojów. Usługa Site Recovery zarządza i organizuje odzyskiwania po awarii maszyn lokalnych i maszyn wirtualnych platformy Azure (maszyny wirtualne), łącznie z replikacji, trybu failover i odzyskiwania różnych obciążeń.

W tym artykule opisano, jak chronić serwer plików przy użyciu usługi Azure Site Recovery i inne zalecenia do potrzeb różnych środowiskach. 

- [Ochrona maszyny serwera plików Azure IaaS](#disaster-recovery-recommendation-for-azure-iaas-virtual-machines)
- [Ochrona serwerów plików lokalnych](#replicate-an-onpremises-file-server-using-azure-site-recovery)


## <a name="file-server-architecture"></a>Architektura serwera plików
W celu otwartego pliku rozproszonej Udostępnianie systemu jest zapewnienie środowisku, w której grupy użytkowników geograficznie rozproszone mogą współpracować wydajnie pracować z plikami i można zagwarantować, że ich wymagania dotyczące integralności są wymuszane. Typowy lokalnymi ekosystemu serwera plików, obsługującego dużej liczby równoczesnych użytkowników i dużą liczbę elementów zawartości na użytek rozproszonych replikacji systemu plików (DFSR) replikacji planowania i ograniczania przepustowości. DFSR stosowany jest algorytm kompresji, znane jako zdalny kompresji (Differential Compression), który może służyć do wydajnie pliki aktualizacji w ograniczonej przepustowości sieci. Ta funkcja wykrywa wstawienia, usuwania i zmianę położenia danych w plikach, włączanie DFSR replikowanie tylko zmienionych bloków pliku podczas aktualizacji plików. Dostępne są także środowisk serwera plików, gdy codzienne kopie zapasowe są wykonywane w chronometrażu poza szczytem, które automatycznie dostosowują się do potrzeb po awarii i nie ma żadnej implementacji usługi DFSR.

Następujące topologii przedstawiono środowisko serwera plików z DFSR zaimplementowana.
                
![DFSR architexture](media/site-recovery-file-server/dfsr-architecture.JPG)

W powyższych odwołanie wielu serwerów plików określonych jako elementy członkowskie, aktywnie uczestniczyć w replikacji plików między grupą replikacji. Zawartość w folderze replikowanym będzie dostępna dla wszystkich klientów wysyłania żądań do jednego z elementów członkowskich, nawet w przypadku awarii jednego z elementów członkowskich przechodzi do trybu offline.

## <a name="disaster-recovery-recommendation-for-file-servers"></a>Zalecenie odzyskiwania po awarii dla serwerów plików:

1.  Replikowanie serwera plików przy użyciu usługi Azure Site Recovery: serwerów plików mogą być replikowane na platformie Azure przy użyciu usługi Azure Site Recovery. W trakcie co najmniej jednego pliku serwery lokalne jest niedostępne, odzyskiwania maszyn wirtualnych mogą zostać przeniesione na platformie Azure, które można następnie służą żądań pochodzących od klientów, w infrastrukturze lokalnej, pod warunkiem że przez połączenie sieci VPN typu lokacja-lokacja i usługę Active directory na platformie Azure. Można to zrobić w przypadku środowiska DFSR skonfigurowane lub środowisku serwera prostego pliku z nie DFSR. 

2.  Rozszerzenie do maszyny Wirtualnej platformy Azure Iaas DFSR:-w środowisku serwera plików z DFSR zaimplementowana jedną zalecane podejście jest rozszerzenie DFSR lokalnego do platformy Azure. Maszyna wirtualna platformy Azure, następnie jest skonfigurowany do przeprowadzania rolę serwera plików. 

    Po zależności połączenie sieci VPN typu lokacja-lokacja i usługi Active directory są obsługiwane i DFSR znajduje się w miejscu, gdy co najmniej jeden plik serwerów lokalnej jest niedostępny, klienci nadal może połączyć się maszyny Wirtualnej Azure, która będzie obsługiwać żądań.

    Takie podejście jest zalecane w przypadku maszyn wirtualnych mają konfiguracje, które nie są obsługiwane przez usługę Azure Site Recovery, tak samo, jak na przykład: dysku udostępnionego klastra, który jest czasami powszechnie używany w środowiskach serwera plików.  DFSR również działa dobrze w środowiskach niskiej przepustowości przy użyciu zmian średnia. Dodatkowych kosztów o maszynie Wirtualnej platformy Azure w i uruchomione przez cały czas również musi znajdować się z tym.  

3.  Użycie usługi synchronizacji plików Azure do replikacji plików: Jeśli przygotowywania dla podróży do chmury, lub już z maszyny Wirtualnej platformy Azure, zalecamy użycie usługi synchronizacji plików platformy Azure, która zapewnia synchronizację z udziałów plików w pełni zarządzana w chmurze, które są dostępne v IA branżowy standard [bloku komunikatów serwera ](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx)protokołu (SMB). Plików na platformę Azure udziałów następnie można zainstalować równolegle przez w chmurze lub lokalnie wdrożeń systemu Windows, Linux i macOS. 

Poniżej diagramu zapewnia graficznie przedstawiają celem łatwiejszym limit decyzji, jakie strategii dla danego środowiska serwera plików.

![drzewa decyzyjnego](media/site-recovery-file-server/decisiontree.png)


### <a name="factors-to-consider-while-making-disaster-recovery-decision"></a>Czynniki, które należy wziąć pod uwagę podczas podejmowania decyzji odzyskiwania po awarii

|Środowisko  |Zalecenie  |Kwestie do rozważenia |
|---------|---------|---------|
|Środowisko serwera lub bez DFSR pliku|   [Użyć usługi Azure Site Recovery na potrzeby replikacji](#replicate-an-onpremises-file-servers-using-azure-site-recovery)   |    Usługa Site Recovery nie obsługuje udostępnionych klastrów dysku, NAS. Jeśli środowisko używa jednej z następujących konfiguracji, użyj jednej z innych metod zależnie od potrzeb. <br> Usługa Azure Site Recovery nie obsługuje protokołu SMB 3.0, co oznacza, że tylko wtedy, gdy zmiany wprowadzone do plików są aktualizowane w oryginalnej lokalizacji pliku będzie zreplikowanej maszyny Wirtualnej wprowadzić zmiany.
|Środowisko serwera plików z DFSR     |  [Rozszerzanie DFSR maszyn wirtualnych IaaS platformy Azure:](#extend-dfsr-to-an-azure-iaas-virtual-machine)  |     DFSR działa dobrze w bardzo przepustowości crunched środowiskach, takie podejście jest jednak wymaga maszyny Wirtualnej platformy Azure w górę i uruchomione przez cały czas. Koszt maszyny wirtualnej musi zostać uwzględnione w procesie planowania.         |
|Maszyna wirtualna platformy Azure Iaas     |     [Synchronizacja plików na platformę Azure](#use-azure-file-sync-service-to-replicate-your-files)   |     W scenariuszu odzyskiwania po awarii Jeśli używasz synchronizacji plików Azure, podczas pracy awaryjnej działań ręcznych należy podjąć w celu zapewnienia, że udziały plików jako dostępne w sposób przezroczysty do komputera klienckiego. AFS wymaga portu 445 być otwarty z komputera klienta.     |


### <a name="site-recovery-support"></a>Obsługa odzyskiwania lokacji
Zgodnie z replikacji usługi Site Recovery jest niezależny od aplikacji, zalecenia zawarte w tym miejscu powinny dotyczy w następujących scenariuszach:
| Element źródłowy    |Do lokacji dodatkowej    |Na platformie Azure
|---------|---------|---------|
|Azure| -|Tak|
|Funkcja Hyper-V|   Tak |Tak
|VMware |Tak|   Tak
|Serwer fizyczny|   Tak |Tak
 

> [!IMPORTANT]
> Przed przystąpieniem do żadnego z poniższych trzech metod, upewnij się, że są podejmowane obsługę następujących zależności:

**Połączenie lokacja-lokacja**: bezpośrednie, aby umożliwić komunikację między serwerami musi można nawiązać połączenia między lokacją lokalną i sieć platformy Azure.  Można to zapewnić za pomocą bezpiecznego połączenia sieci VPN typu lokacja-lokacja, do sieci wirtualnej systemu Windows Azure, która będzie służyć jako lokacja DR.  
Zobacz: [VPN lokacja-lokacja ustanowienie połączenia między lokacją lokalną i sieć platformy Azure](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

**Usługi Active Directory**: DFSR zależy od usługi Active Directory.  Oznacza to, że lasu usługi Active Directory za pomocą lokalnych kontrolerów domeny jest rozszerzony do lokacji odzyskiwania po awarii na platformie Azure. Nawet jeśli nie używasz DFSR, jeśli uprawnionych użytkowników muszą być przyznane / zweryfikować dostępu, takich jak w większości organizacji, należy wykonać następujące kroki.
Zobacz: [rozszerzyć w lokalnej usłudze Active Directory na platformie Azure](https://docs.microsoft.com/azure/site-recovery/site-recovery-active-directory).

## <a name="disaster-recovery-recommendation-for-azure-iaas-virtual-machines"></a>Zalecenie odzyskiwania po awarii dla maszyn wirtualnych IaaS platformy Azure

W przypadku konfigurowania i zarządzania odzyskiwania po awarii hostowanych na maszynach wirtualnych Azure IaaS, można wybrać jedną z dwóch opcji, serwerów plików w oparciu czy chcesz przenieść [plików Azure](https://docs.microsoft.com/azure/storage/files/storage-files-introduction).

1. [Synchronizację plików na platformę Azure](#use-azure-file-sync-service-to-replicate-files-hosted-on-iaas-virtual-machine)
2. [Korzystanie z usługi Azure Site Recovery](#replicate-an-iaas-file-server-virtual-machine-using-azure-site-recovery)

## <a name="use-azure-file-sync-service-to-replicate-files-hosted-on-iaas-virtual-machine"></a>Użyj usługi synchronizacji plików Azure replikację plików obsługiwanych na maszynie wirtualnej modelu IaaS

**Usługa pliki Azure** można całkowicie zastępujące lub uzupełniające, serwerów plików tradycyjnych, lokalnie lub urządzeniach NAS. Udziały plików platformy Azure można również replikować za pomocą usługi Azure File Sync na serwerach z systemem Windows, lokalnie lub w chmurze, w celu zapewnienia wydajnego i rozproszonego buforowania danych, gdziekolwiek są używane. Kroki szczegółowo zalecenie dotyczące odzyskiwania po awarii dla maszyn wirtualnych platformy Azure, który wykonywać takie same funkcje jak tradycyjnych serwerów plików:
1.  Ochrona komputerów za pomocą usługi Azure Site Recovery przy użyciu czynności wymienionych w tym miejscu
2.  Synchronizacja plików Azure umożliwia replikację plików z maszyny Wirtualnej, który działa jako serwer plików do chmury.
3.  Użyj funkcji planu odzyskiwania usługi Azure Site Recovery w celu dodania skryptów do [udziałów plików Azure](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) i uzyskania dostępu do udziału na maszynie wirtualnej.

Kroki krótko opisano sposób korzystania z usługi synchronizacji plików Azure:

1. [Utwórz konto magazynu na platformie Azure](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). W przypadku wybrania dostęp do odczytu magazynu geograficznie nadmiarowego (RA-GRS) dla kont magazynu otrzymasz dostęp do odczytu do danych z regionu pomocniczego w przypadku awarii. Zapoznaj się z [strategii odzyskiwania danych udział plików Azure](https://docs.microsoft.com/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) uzyskać dalsze informacje.

2. [Tworzenie udziału plików](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share)

3. [Wdrażanie synchronizacji plików Azure](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) na serwerze plików na platformę Azure.

4. Utwórz grupę synchronizacji: punkty końcowe w ramach grupy synchronizacji będą utrzymywane w synchronizacji ze sobą. Grupy synchronizacji musi zawierać co najmniej jednej chmurze punktu końcowego, który reprezentuje udział plików Azure, i co serwer punktu końcowego, który reprezentuje ścieżkę, w systemie Windows Server.
5.  Pliki będą teraz są synchronizowane między udziału plików platformy Azure i na serwerze lokalnym.
6.  W przypadku awarii w środowisku lokalnym, należy wykonać jako tryb failover przy użyciu planu odzyskiwania i Dodaj skrypt [udziałów plików Azure](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) i uzyskania dostępu do udziału na maszynie wirtualnej.

### <a name="replicate-an-iaas-file-server-virtual-machine-using-azure-site-recovery"></a>Replikowanie IaaS plików maszyny wirtualnej serwera przy użyciu usługi Azure Site Recovery

Jeśli masz na klientach lokalnych podczas uzyskiwania dostępu do IaaS maszyny wirtualnej serwera plików, wykonaj najpierw 2 kroki również else przejdź do kroku 3.

1. Nawiąż połączenie sieci VPN typu lokacja-lokacja między lokacją lokalną i sieć platformy Azure.  
2. Rozszerzenie w lokalnej usłudze Active Directory.
3. [Konfigurowanie odzyskiwania po awarii](azure-to-azure-tutorial-enable-replication.md) IaaS maszyny serwera plików w regionie pomocniczym.


Więcej informacji dotyczących odzyskiwania po awarii w regionie pomocniczym, można znaleźć [tutaj](concepts-azure-to-azure-architecture.md).


## <a name="replicate-an-on-premises-file-server-using-azure-site-recovery"></a>Replikowanie lokalnych serwera plików przy użyciu usługi Azure Site Recovery
Poniżej czynności szczegóły replikacji dla maszyny Wirtualnej VMware, kroki do replikowania maszyn wirtualnych funkcji Hyper-V, można znaleźć tutaj.

1.  [Przygotowanie zasobów Azure](tutorial-prepare-azure.md) dla replikację maszyn lokalnych.
2.  Nawiąż połączenie sieci VPN typu lokacja-lokacja między lokacją lokalną i sieć platformy Azure.  
3. Rozszerzenie w lokalnej usłudze Active Directory.
4.  [Przygotowanie serwerów VMware lokalnych](tutorial-prepare-on-premises-vmware.md).
5.  [Konfigurowanie odzyskiwania po awarii](tutorial-vmware-to-azure.md) Azure dla maszyn wirtualnych lokalnie.

## <a name="extend-dfsr-to-an-azure-iaas-virtual-machine"></a>Rozszerzanie DFSR maszyn wirtualnych IaaS platformy Azure:

1.  Nawiąż połączenie sieci VPN typu lokacja-lokacja między lokacją lokalną i sieć platformy Azure. 
2.  Rozszerzenie w lokalnej usłudze Active Directory.
3.  [Tworzenie i Inicjowanie ich obsługi serwera plików maszyny Wirtualnej](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) w sieci wirtualnej systemu Windows Azure.

    Upewnij się, czy maszyna wirtualna została dodana do tego samego systemu Windows Azure wirtualnych sieci, co ma krzyżowego łączność ze środowiska lokalnego. 

4.  Zainstaluj i [Konfigurowanie replikacji systemu plików DFS](https://blogs.technet.microsoft.com/b/filecab/archive/2013/08/21/dfs-replication-initial-sync-in-windows-server-2012-r2-attack-of-the-clones.aspx) w systemie Windows Server.

5.  [Implementowanie Namespace systemu plików DFS](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/deploying-dfs-namespaces).
6.  Z Namespace systemu plików DFS, zaimplementowana pracy awaryjnej folderów udostępnionych z produkcji do odzyskiwania po awarii lokacji może odbywać się przez zaktualizowanie obiektów docelowych folderów systemu plików DFS Namespace.  Po replikacji zmiany Namespace systemu plików DFS za pomocą usługi Active Directory, użytkownicy są połączeni cele odpowiedni folder przezroczysty.

## <a name="use-azure-file-sync-service-to-replicate-your-on-premises-files"></a>Użyj usługi synchronizacji plików Azure w celu zreplikowania plików lokalnych:
Przy użyciu usługi synchronizacji plików Azure można replikować pożądanych plików w chmurze tak, aby w przypadku awarii i niedostępność serwera plików lokalnie, można zainstalować lokalizacje żądanego pliku z chmury do i kontynuować do obsługi żądań z na komputerach klienckich.
Sugerowane rozwiązania integracji Azure plik synchronizacji z usługą Azure Site Recovery
1.  Ochrona maszyny serwera plików przy użyciu usługi Azure Site Recovery przy użyciu czynności wymienionych [tutaj](tutorial-vmware-to-azure.md)
2.  Synchronizacja plików Azure umożliwia replikację plików z komputera, który służy jako serwer plików do chmury.
3.  Użyj funkcji planu odzyskiwania usługi Azure Site Recovery, aby dodać skryptów do zainstalowania udziału plików platformy Azure na nieudane za pośrednictwem plików wirtualna na platformie Azure.

Poniżej szczegółów czynności przy użyciu usługi Azure synchronizacji plików:

1. [Utwórz konto magazynu na platformie Azure](https://docs.microsoft.com/en-us/azure/storage/common/storage-create-storage-account?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Jeśli wybrano magazyn geograficznie nadmiarowy dostęp do odczytu (RA-GRS) (zalecane) w przypadku kont magazynu, konieczne będzie dostęp do odczytu danych z regionu pomocniczego w przypadku awarii. Zapoznaj się z [strategii odzyskiwania danych udział plików Azure](https://docs.microsoft.com/en-us/azure/storage/common/storage-disaster-recovery-guidance?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) uzyskać dalsze informacje.

2. [Tworzenie udziału plików](https://docs.microsoft.com/azure/storage/files/storage-how-to-create-file-share)

3. [Wdrażanie synchronizacji plików Azure](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide) w lokalnej serwera plików.

4. Utwórz grupę synchronizacji: punkty końcowe w ramach grupy synchronizacji będą utrzymywane w synchronizacji ze sobą. Grupy synchronizacji musi zawierać co najmniej jednej chmurze punktu końcowego, który reprezentuje udział plików Azure, i co serwer punktu końcowego, który reprezentuje ścieżkę, w systemie Windows Server lokalnymi.

1. Pliki będą teraz są synchronizowane między udziału plików platformy Azure i na serwerze lokalnym.
6.  W przypadku awarii w środowisku lokalnym należy wykonać jako tryb failover przy użyciu planu odzyskiwania i dodać skrypt, aby zainstalować udział plików Azure i uzyskać dostępu do udziału na maszynie wirtualnej.

> [!NOTE]
> Upewnij się, jest otwarty port 445: pliki Azure korzysta z protokołu SMB. Protokół SMB komunikuje się za pośrednictwem portu TCP 445. Upewnij się, że Twoja zapora nie blokuje portów TCP 445 z komputera klienckiego.


## <a name="doing-a-test-failover"></a>Ten test trybu failover

1.  Przejdź do portalu Azure i wybierz magazyn usług odzyskiwania.
2.  Kliknij utworzony dla środowiska plików planu odzyskiwania.
3.  Kliknij na "Test trybu Failover".
4.  Wybierz punkt odzyskiwania i sieć wirtualna platformy Azure, aby rozpocząć proces test trybu failover.
5.  Po skonfigurowaniu dodatkowej środowiska można wykonywać z operacji sprawdzania poprawności.
6.  Po zakończeniu operacji sprawdzania poprawności, kliknij przycisk "Oczyszczania testowy tryb failover" w planie odzyskiwania i testowe środowisko trybu failover jest czyszczona.

Aby uzyskać więcej informacji dotyczących wykonywania test trybu failover odnoszą się [tutaj](site-recovery-test-failover-to-azure.md).

Aby uzyskać wskazówki na ten test trybu failover dla usługi AD i DNS, zapoznaj się z [test pracy awaryjnej zagadnienia dotyczące AD i DNS](site-recovery-active-directory.md).

## <a name="doing-a-failover"></a>Podczas pracy w trybie failover

1.  Przejdź do portalu Azure i wybierz magazyn usług odzyskiwania.
2.  Kliknij utworzony dla środowiska plików planu odzyskiwania.
3.  Kliknij pozycję "Failover".
4.  Wybierz punkt odzyskiwania, aby rozpocząć proces trybu failover.

Aby uzyskać więcej informacji dotyczących wykonywania test trybu failover odnoszą się [tutaj](site-recovery-failover.md).
