---
title: 'Site Recovery: często zadawane pytania | Microsoft Docs'
description: Ten artykuł zawiera odpowiedzi na często zadawane pytania dotyczące usługi Azure Site Recovery.
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: jwhit
editor: ''

ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/27/2016
ms.author: raynew

---
# Azure Site Recovery: często zadawane pytania (FAQ)
Przeczytaj ten artykuł, aby zapoznać się z często zadawanymi pytaniami dotyczącymi usługi Azure Site Recovery.

Jeśli po przeczytaniu artykułu nadal masz pytania, możesz zamieścić je pod tym artykułem lub na [forum Usług odzyskiwania Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).

## Ogólne
### Do czego służy usługa Site Recovery?
Usługa Site Recovery przyczynia się do strategii związanej z ciągłością biznesową i odzyskiwaniem po awarii (BCDR, business continuity and disaster recovery) poprzez organizowanie i automatyzowanie replikacji z lokalnych maszyn wirtualnych i serwerów fizycznych do platformy Azure lub dodatkowego centrum danych. [Dowiedz się więcej](site-recovery-overview.md).

### Co może replikować usługa Site Recovery?
* **Maszyny wirtualne funkcji Hyper-V**: usługa Site Recovery może replikować dowolne obciążenia uruchomione na obsługiwanej maszynie wirtualnej funkcji Hyper-V. 
* **Serwery fizyczne**: usługa Site Recovery może replikować dowolne obciążenia uruchomione na obsługiwanym serwerze fizycznym z systemem Windows/Linux.
* **Maszyny wirtualne VMware**: usługa Site Recovery może replikować dowolne obciążenia uruchomione na obsługiwanej maszynie wirtualnej VMware.

### Czego potrzebuję w funkcji Hyper-V?
Wymagania serwera hosta funkcji Hyper-V zależą od scenariusza wdrożenia. Zapoznaj się z wymaganiami wstępnymi funkcji Hyper-V:

* [Replicating Hyper-V VMs (without VMM) to Azure (Replikacja maszyn wirtualnych funkcji Hyper-V [bez programu VMM] do platformy Azure)](site-recovery-hyper-v-site-to-azure.md#before-you-start)
* [Replicating Hyper-V VMs (with VMM) to Azure (Replikacja maszyn wirtualnych funkcji Hyper-V [z programem VMM] do platformy Azure)](site-recovery-vmm-to-azure.md#before-you-start)
* [Replicating Hyper-V VMs to a secondary datacenter (Replikacja maszyn wirtualnych funkcji Hyper-V do dodatkowego centrum danych)](site-recovery-vmm-to-vmm.md#before-you-start)

W przypadku gości na serwerze hosta funkcji Hyper-V:

* Jeśli przeprowadzasz replikację do lokacji dodatkowej, przeczytaj o [obsługiwanych systemach operacyjnych gościa dla maszyn wirtualnych funkcji Hyper-V](https://technet.microsoft.com/library/mt126277.aspx).
* Jeśli przeprowadzasz replikację do platformy Azure, usługa Site Recovery obsługuje te same systemy operacyjne gościa, które są [obsługiwane na platformie Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).

### Czy mogę chronić maszyny wirtualne, jeśli funkcja Hyper-V jest uruchomiona na systemie operacyjnym klienta?
Nie. Maszyny wirtualne muszą znajdować się na serwerze hosta funkcji Hyper-V, który jest uruchomiony na serwerze z obsługiwanym systemem Windows. Jeśli musisz chronić komputer kliencki, możesz replikować go jako maszynę fizyczną [do platformy Azure](site-recovery-vmware-to-azure-classic.md) lub do [dodatkowego centrum danych](site-recovery-vmware-to-vmware.md).

### Jakie obciążenia mogę chronić za pomocą usługi Site Recovery?
Usługa Site Recovery może replikować dowolne obciążenia uruchomione na obsługiwanej maszynie wirtualnej lub serwerze fizycznym. Usługa Site Recovery zapewnia wsparcie dla replikacji obsługującej aplikacje, dzięki czemu aplikacje mogą być odzyskiwane do inteligentnego stanu. Usługa integruje się z aplikacjami firmy Microsoft, np. SharePoint, Exchange, Dynamics, SQL Server i Active Directory, oraz ściśle współpracuje z wiodącymi dostawcami oprogramowania, w tym firmami Oracle, SAP, IBM i Red Hat. [Dowiedz się więcej](site-recovery-workload.md) o ochronie obciążeń.

### Czy hosty funkcji Hyper-V muszą znajdować się w chmurach System Center VMM, aby replikować je przy użyciu usługi Site Recovery?
Jeśli chcesz replikować do dodatkowego centrum danych, maszyny wirtualne funkcji Hyper-V muszą znajdować się na serwerach hostów funkcji Hyper-V w chmurze VMM. Jeśli chcesz replikować do platformy Azure, możesz replikować maszyny wirtualne na serwerach hostów funkcji Hyper-V z wykorzystaniem chmur VMM lub bez nich. [Dowiedz się więcej](site-recovery-hyper-v-site-to-azure.md). 

### Czy mogę wdrożyć usługę Site Recovery z programem VMM, jeśli mam tylko jeden serwer VMM?
Tak. Możesz replikować maszyny wirtualne na serwerach funkcji Hyper-V w chmurze VMM do platformy Azure lub replikować je między chmurami VMM na tym samym serwerze. W przypadku replikacji lokalnej zalecamy posiadanie serwera VMM w obu lokacjach: głównej i dodatkowej. [Dowiedz się więcej](site-recovery-single-vmm.md)

### Jakie serwery fizyczne mogę chronić?
Możesz chronić serwery fizyczne z obsługiwanymi systemami operacyjnymi Windows lub Linux na platformie Azure lub w lokacji dodatkowej. [Dowiedz się](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) o wymaganiach systemu operacyjnego dla replikacji do platformy Azure lub lokacji dodatkowej. Pamiętaj, że serwery fizyczne replikowane do magazynu Azure będą działać jako maszyny wirtualne na platformie Azure, jeśli nastąpi przejście do trybu failover. W przypadku powrotu po awarii z platformy Azure do lokacji lokalnej powrót do fizycznego serwera nie jest obecnie obsługiwany. Możesz wykonać wyłącznie powrót po awarii do maszyny wirtualnej VMware.

### Jakie maszyny wirtualne VMware mogę chronić?
Aby chronić maszyny wirtualne VMware, należy posiadać program vSphere Hypervisor i maszyny wirtualne z uruchomionymi narzędziami VMware. Zalecamy również posiadanie serwera VMware vCenter do zarządzania funkcjami hypervisor. [Dowiedz się więcej](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment) o dokładnych wymaganiach dotyczących replikacji serwerów i maszyn wirtualnych VMware do platformy Azure lub lokacji dodatkowej.

### Czy mogę zarządzać odzyskiwaniem po awarii dla oddziałów firmy przy użyciu usługi Site Recovery?
Tak. Gdy używasz usługi Site Recovery do organizowania replikacji i trybu failover dla oddziałów firmy, możesz organizować i wyświetlać wszystkie obciążenia oddziałów w lokacji centralnej. Możesz z łatwością uruchomić tryb failover i zarządzać odzyskiwaniem po awarii wszystkich oddziałów z siedziby, bez konieczności odwiedzania oddziałów. 

## Bezpieczeństwo
### Czy dane replikacji są wysyłane do usługi Site Recovery?
Nie. Usługa Site Recovery nie przechwytuje replikowanych danych ani nie ma żadnych informacji o tym, co jest uruchomione na Twoich maszynach wirtualnych lub serwerach fizycznych.

Dane replikacji są wymieniane między lokalnymi hostami funkcji Hyper-V, funkcjami hypervisor VMware lub serwerami fizycznymi i magazynem Azure lub lokacją dodatkową.  Usługa Site Recovery nie ma możliwości przechwycenia tych danych. Do usługi Site Recovery są wysyłane jedynie metadane wymagane do organizowania replikacji i trybu failover. 

Usługa Site Recovery ma certyfikat ISO 27001:2005 i obecnie trwa proces pozyskiwania ocen HIPAA, DPA i FedRAMP JAB.

### Aby zapewnić zgodność, nawet metadane pochodzące z naszych lokalnych środowisk muszą pozostać w tym samym regionie geograficznym. Czy usługa Site Recovery może to zapewnić?
Tak. Gdy utworzysz magazyn usługi Site Recovery w danym regionie, upewniamy się, że wszystkie metadane wymagane do uruchamiania i organizowania replikacji i trybu failover pozostaną w granicach tego regionu geograficznego.

### Czy usługa Site Recovery szyfruje replikację?
W przypadku replikacji maszyn wirtualnych i serwerów fizycznych między lokacjami lokalnymi obsługiwane jest szyfrowanie podczas przesyłania danych. W przypadku replikacji maszyn wirtualnych i serwerów fizycznych do platformy Azure obsługiwane jest szyfrowanie podczas przesyłania danych i szyfrowanie danych w stanie spoczynku (na platformie Azure). 

## Replikacja
### Czy istnieją wymagania wstępne związane z replikacją maszyn wirtualnych do platformy Azure?
Maszyny wirtualne, które mają zostać replikowane do platformy Azure muszą być zgodne z [wymaganiami platformy Azure](site-recovery-best-practices.md#azure-virtual-machine-requirements).

### Czy można replikować maszyny wirtualne funkcji Hyper-V drugiej generacji do platformy Azure?
Tak. Usługa Site Recovery konwertuje maszyny wirtualne drugiej generacji do pierwszej generacji podczas pracy w trybie failover. W momencie powrotu po awarii maszyna wirtualna jest konwertowana z powrotem do drugiej generacji. [Dowiedz się więcej](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).

### W przypadku replikacji do platformy Azure, jak będę płacić za maszyny wirtualne Azure?
Podczas regularnej replikacji dane są replikowane do magazynu geograficznie nadmiarowego Azure i nie trzeba wnosić jakichkolwiek opłat za maszynę wirtualną Azure w modelu IaaS.  Po uruchomieniu trybu failover na platformie Azure usługa Site Recovery automatycznie utworzy maszyny wirtualne Azure w modelu IaaS. Po tym zostaną naliczone opłaty za zasoby obliczeniowe, które zostaną zużyte na platformie Azure.

### Czy istnieje zestaw SDK, którego mogę użyć do zautomatyzowania przepływów pracy usługi Site Recovery?
Tak. Przepływy pracy usługi Site Recovery można zautomatyzować przy użyciu interfejsu API REST, programu PowerShell lub zestawu SDK platformy Azure. Dowiedz się więcej o [wdrażaniu usługi Site Recovery przy użyciu programów PowerShell i Azure Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md).

### Jeśli przeprowadzam replikację do platformy Azure, jakiego konta magazynu potrzebuję?
Potrzebujesz konta magazynu ze [standardowym magazynem geograficznie nadmiarowym](../storage/storage-introduction.md#replication-for-durability-and-high-availability). Usługa Premium Storage nie jest obecnie obsługiwana.

### Jak często mogę replikować dane?
* **Maszyny wirtualne Hyper-V:** maszyny wirtualne funkcji Hyper-V w systemie Windows Server 2012 R2 mogą być replikowane co 30 sekund, 5 minut lub 15 minut. Po skonfigurowaniu replikacji sieci SAN replikacja będzie odbywać się synchronicznie.
* **Maszyny wirtualne VMware i serwery fizyczne:** częstotliwość replikacji nie jest tutaj istotna. Replikacja będzie ciągła. 

### Czy mogę rozszerzyć replikację z istniejącej lokacji odzyskiwania na kolejną lokację odzyskiwania?
Replikacja rozszerzona lub łańcuchowa nie jest obsługiwana. [Prześlij nam opinię](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication/) o tej funkcji.

### Czy mogę przeprowadzić replikację w trybie offline podczas pierwszej replikacji do platformy Azure?
Ta funkcja nie jest obsługiwana. [Prześlij nam opinię](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from/) o tej funkcji.

### Czy z replikacji można wykluczyć określone dyski?
Ta funkcja nie jest obsługiwana. [Prześlij nam opinię](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6418801-exclude-disks-from-replication/) o tej funkcji.

### Czy można replikować maszyny wirtualne z dyskami dynamicznymi?
Dyski dynamiczne są obsługiwane w przypadku replikacji maszyn wirtualnych funkcji Hyper-V. Są one również obsługiwane podczas replikowania maszyn wirtualnych VMware i maszyn fizycznych w przypadku użycia [wdrożenia rozszerzonego](site-recovery-vmware-to-azure-classic.md). Pamiętaj, że dysk systemu operacyjnego musi być dyskiem podstawowym.

### Czy mogę ograniczyć przepustowość przydzieloną dla ruchu replikacji funkcji Hyper-V?
* W przypadku replikacji maszyn wirtualnych funkcji Hyper-V między dwoma lokacjami lokalnymi możesz użyć usługi Windows QoS. Poniżej przedstawiono przykładowy skrypt: 
  
        New-NetQosPolicy -Name ASRReplication -IPDstPortMatchCondition 8084 -ThrottleRate (2048*1024)
        gpupdate.exe /force
* Jeśli replikujesz maszyny wirtualne funkcji Hyper-V do platformy Azure, możesz skonfigurować ograniczanie przepustowości przy użyciu następującego, przykładowego polecenia cmdlet programu PowerShell:
  
        Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)
* [Dowiedz się więcej](https://support.microsoft.com/en-us/kb/3056159) o ograniczaniu przepustowości ruchu funkcji Hyper-V.
* [Dowiedz się więcej](site-recovery-vmware-to-azure-classic.md#capacity-planning) o ograniczaniu przepustowości replikacji maszyn wirtualnych VMware do platformy Azure.

## Tryb failover
### Jak uzyskać dostęp do maszyn wirtualnych Azure po przejściu w tryb failover na platformie Azure?
Maszyny wirtualne Azure są dostępne za pośrednictwem bezpiecznego połączenia internetowego, sieci VPN między lokacjami lub za pośrednictwem usługi Azure ExpressRoute. Komunikacja za pośrednictwem połączenia VPN jest prowadzona do wewnętrznych portów w sieci Azure, w której znajduje się maszyna wirtualna. Komunikacja za pośrednictwem Internetu jest mapowana na publiczne punkty końcowe w usłudze w chmurze Azure dla maszyn wirtualnych.

### W przypadku trybu failover na platformie Azure, w jaki sposób platforma Azure upewnia się, że moje dane są odporne?
Platforma Azure została zaprojektowana z myślą o odporności danych. Usługa Site Recovery jest już zaprojektowana z myślą o pracy w trybie failover w dodatkowym centrum danych Azure, zgodnie z warunkami umowy SLA platformy Azure, jeśli zajdzie taka potrzeba. W takim przypadku upewniamy się, że Twoje metadane i magazyny pozostają w tym samym regionie geograficznym, który został wybrany dla magazynu. 

### Jeśli przeprowadzam replikację między dwiema lokacjami, co się stanie, gdy wystąpi nieoczekiwana awaria w lokacji głównej?
Możesz wyzwolić nieplanowany tryb failover z lokacji dodatkowej. Usługa Site Recovery nie wymaga łączności z lokacją główną do pracy w trybie failover.

### Czy tryb failover jest automatyczny?
Tryb failover nie jest automatyczny. Tryb failover inicjuje się jednym kliknięciem w portalu. Możesz też użyć [poleceń cmdlet programu PowerShell związanych z usługą Site Recovery](https://msdn.microsoft.com/library/dn850420.aspx), aby wyzwolić tryb failover. Powrót po awarii jest serią działań w portalu usługi Site Recovery.

Aby zautomatyzować tryb failover, możesz użyć lokalnego narzędzia Orchestrator lub programu Operations Manager, aby wykryć awarię maszyny wirtualnej i wyzwolić tryb failover przy użyciu zestawu SDK.

## 
### Jestem dostawcą usług. Czy usługa Site Recovery działa w przypadku modeli opartych na infrastrukturze dedykowanej lub współdzielonej?
Tak. Usługa Site Recovery obsługuje modele oparte na infrastrukturze dedykowanej i współdzielonej.

### W przypadku dostawy usług, czy tożsamość mojej dzierżawy jest udostępniana usłudze Site Recovery?
Nie. Twoje dzierżawy nie wymagają dostępu do portalu usługi Site Recovery. Tylko administrator dostawcy usług wykonuje czynności w portalu.

### Czy dane aplikacji moich dzierżaw trafią kiedykolwiek do platformy Azure?
Podczas replikacji między lokacjami należącymi do dostawcy usług dane aplikacji nigdy nie trafiają do platformy Azure. Dane są szyfrowane podczas przesyłania i replikowane bezpośrednio między lokacjami dostawcy usług.

Jeśli przeprowadzasz replikację do platformy Azure, dane aplikacji są wysyłane do magazynu Azure, ale nie do usługi Site Recovery. Dane są szyfrowane podczas przesyłania i pozostają zaszyfrowane na platformie Azure. 

### Czy moje dzierżawy otrzymają rachunek za jakiekolwiek usługi platformy Azure?
Nie. Relacja rozliczeniowa platformy Azure jest nawiązana bezpośrednio z dostawcą usług. Dostawcy usług są odpowiedzialni za generowanie konkretnych rachunków dla swoich dzierżaw.

### Jeśli przeprowadzam replikację do platformy Azure, czy moje maszyny wirtualne muszą być uruchomione na platformie Azure przez cały czas?
Nie. Dane są replikowane do konta geograficznie nadmiarowego magazynu Azure w ramach subskrypcji. Podczas testowego (test odzyskiwania po awarii) lub rzeczywistego uruchamiania trybu failover usługa Site Recovery automatycznie tworzy maszyny wirtualne w ramach subskrypcji.

### Czy podczas replikacji do platformy Azure jest zapewniana izolacja na poziomie dzierżawy?
Tak.

### Jakie platformy są obecnie obsługiwane?
Obsługujemy wdrożenia oparte na pakiecie Azure Pack, systemie Cloud Platform System i programie System Center (wersja 2012 lub nowsza). Dowiedz się więcej na temat integracji pakietu Azure Pack w temacie [Configure protection for virtual machines](https://technet.microsoft.com/library/dn850370.aspx) (Konfiguracja ochrony dla maszyn wirtualnych).

### Czy są obsługiwane wdrożenia oparte na jednym pakiecie Azure Pack i jednym serwerze VMM?
Tak. Możesz replikować maszyny wirtualne funkcji Hyper-V i platformy Azure lub replikować między lokacjami dostawcy usług.  Pamiętaj, że jeśli replikujesz między lokacjami dostawcy usług, nie jest dostępna integracja elementów runbook platformy Azure.

## Następne kroki
* Zapoznaj się z [omówieniem usługi Site Recovery](site-recovery-overview.md)
* Dowiedz się więcej o [architekturze usługi Site Recovery](site-recovery-components.md)  

<!--HONumber=Jun16_HO2-->


