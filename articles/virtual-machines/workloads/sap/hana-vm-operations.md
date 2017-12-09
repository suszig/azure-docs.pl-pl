---
title: SAP HANA operacje na platformie Azure | Dokumentacja firmy Microsoft
description: Operacje SAP HANA na maszynach wirtualnych Azure natywnego
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: juergent
manager: patfilot
editor: 
tags: azure-resource-manager
keywords: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/17/2017
ms.author: msjuergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ab609fe9e7b01d7087dd00c22c19e69a471f6599
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="sap-hana-on-azure-operations-guide"></a>SAP HANA w podręczniku obsługi platformy Azure
Ten przewodnik zawiera wskazówki dotyczące systemów SAP HANA, które zostały wdrożone na maszynach wirtualnych platformy Azure. Ten dokument nie ma zastąpić dowolne standardowe dokumentacji SAP. Przewodniki programu SAP i informacje można znaleźć w następujących lokalizacjach:

- [Przewodnik administracji programu SAP](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [Przewodnik po instalacji programu SAP](https://service.sap.com/instguides)
- [Uwaga SAP](https://sservice.sap.com/notes)

Warunkiem wstępnym jest, że dysponujesz podstawową wiedzą na różne składniki platformy Azure:

- [Maszyny wirtualne platformy Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Sieć platformy Azure i sieci wirtualnych](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Storage](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Dodatkową dokumentację SAP NetWeaver i inne składniki SAP na platformie Azure można znaleźć w [SAP na platformie Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) sekcji [dokumentacji platformy Azure](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Zagadnienia dotyczące konfiguracji
### <a name="connecting-into-azure"></a>Łączenie na platformie Azure
Zgodnie z opisem w [maszyny wirtualne Azure planowania i wdrażania dla programu SAP NetWeaver] [-Podręcznik planowania], istnieją dwie podstawowe metody połączenie do maszyn wirtualnych Azure. 

- Łączenie za pośrednictwem Internetu i publiczne punkty końcowe na przejście maszyny Wirtualnej lub maszyny Wirtualnej uruchomionej SAP HANA
- Łączących się za pośrednictwem [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) lub Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/)

W środowisku produkcyjnym scenariuszy lub scenariuszy których nieprodukcyjnych scenariusze źródła danych do produkcji scenariuszy w połączeniu z oprogramowania SAP, należy mieć połączenie lokacja lokacja, za pośrednictwem sieci VPN lub usługi ExpressRoute, jak pokazano na rysunku:

![Łączność między lokacjami](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choice-of-azure-vm-types"></a>Wybór typu maszyny Wirtualnej Azure
Można przeszukiwać Azure typach maszyn wirtualnych, które mogą być używane w scenariuszach produkcji [tutaj](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). W przypadku scenariuszy nieprodukcyjnych można szeroką gamę macierzysty maszynach wirtualnych platformy Azure. Jednak należy ograniczyć się do typów maszyny Wirtualnej, które są [SAP Uwaga #1928533](https://launchpad.support.sap.com/#/notes/1928533). Wdrożenie tych maszyn wirtualnych na platformie Azure można zrobić za pomocą:

- Azure Portal
- Polecenia cmdlet programu Powershell systemu Azure
- Interfejs wiersza polecenia platformy Azure

Również można wdrożyć pełną zainstalowanej na usługi maszyny wirtualne Azure, za pośrednictwem platformy SAP HANA [platformy w chmurze SAP](https://cal.sap.com/) zgodnie z opisem [tutaj](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="choice-of-azure-storage"></a>Wybór magazynu Azure
Platforma Azure oferuje dwa typy magazynu głównego odpowiedni w przypadku maszyn wirtualnych platformy Azure systemem SAP HANA

- [Azure Standard Storage](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)
- [Magazyn w warstwie Premium systemu Azure](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)

System Azure oferuje dwie metody wdrażania dla dysków VHD na Azure Standard i Premium Storage. Pozwala na ogólną scenariusz, zaleca się wykorzystać [dysku zarządzanego Azure](https://azure.microsoft.com/services/managed-disks/) wdrożeń.

Typy magazynu dokładne i umów SLA wokół tych typów magazynu można znaleźć [tej dokumentacji](https://azure.microsoft.com/pricing/details/managed-disks/)

Zaleca się używać dysków Premium Azure /hana/data i /hana/log woluminów. Możliwe jest tworzenie RAID LVM przez wiele dysków Premium Storage i użyć tych woluminu RAID jako /hana/data oraz /hana/log woluminu.

Możliwych konfiguracji dla różnych typowych typów maszyny Wirtualnej, które klientów do tej pory używane do obsługi SAP HANA na maszynach wirtualnych Azure może wyglądać jak:

| JEDNOSTKA SKU MASZYNY WIRTUALNEJ | Pamięć RAM | / hana/danych i dziennika/hana /<br /> paski LVM lub MDADM | / hana/udostępnionych | wolumin/root | / usr/sap | Hana lub tworzenia kopii zapasowej |
| --- | --- | --- | --- | --- | --- | -- |
| E16v3 | 128GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S10 |
| E32v3 | 256GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 443GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 GB | 2 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M64s | 1TB | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1.7 TB | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2TB | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128ms | 3.8 TB | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 5 x S30 |


### <a name="azure-networking"></a>Sieć platformy Azure
Przy założeniu, że masz sieci VPN lub ExpressRoute połączenie lokacja lokacja na platformie Azure, co najmniej czy posiadasz [sieci wirtualnej Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) podłączonego za pośrednictwem bramy sieci wirtualnej z obwodem sieci VPN lub usługi ExpressRoute. Wirtualna brama znajduje się w podsieci w sieci wirtualnej platformy Azure. Aby można było zainstalować HANA, należy utworzyć inny dwie podsieci w sieci wirtualnej. Czy hosty wirtualne, które uruchomione wystąpienia SAP HANA i innej podsieci, która uruchamia ostatecznego Jumpbox lub wirtualne zarządzania, który może obsługiwać SAP HANA Studio lub inne oprogramowanie do zarządzania jedną podsieć.
Po zainstalowaniu maszyn wirtualnych, które należy uruchomić HANA powinny mieć maszyn wirtualnych:

- Dwie wirtualne karty sieciowe zainstalowane z których jeden łączy się z podsieci zarządzania i jednej karcie Sieciowej jest używany do łączenia z zarówno na lokalnym lub w innych sieciach z wystąpieniem SAP HANA w maszynie Wirtualnej platformy Azure.
- Statyczne prywatnych adresów IP wdrożonych dla obu vNICs

Omówienie różnych możliwości przypisywania adresów IP można znaleźć [tutaj](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

Routing ruchu do bezpośrednio do wystąpienia SAP HANA lub jumpbox jest kierowany przez [grup zabezpieczeń sieci Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) skojarzonych podsieci HANA i podsieci zarządzania.

Ogólny schemat nierównej wdrożenia wyglądałyby tak jak:

![Schemat nierównej wdrożenia SAP HANA](media/hana-vm-operations/hana-simple-networking.PNG)


Tylko w przypadku wdrożenia SAP HANA na platformie Azure, bez konieczności lokacja lokacja (sieć VPN lub ExpressRoute na platformie Azure), chociaż publiczny adres IP, który jest przypisany do maszyny Wirtualnej Azure z systemem Jumpbox maszyny Wirtualnej może uzyskać dostęp z wystąpieniem SAP HANA. W przypadku prostego możesz również polegać na wbudowanych usług DNS platformy Azure w celu rozpoznania nazwy hostów. Szczególnie, gdy przy użyciu publiczne adresy IP połączonej, chcesz użyć grup zabezpieczeń sieci Azure, aby ograniczyć otwartych portów lub zakresy adresów IP, które mogą łączyć się w podsieci platformy Azure z uruchomionymi zasoby z publicznych adresów IP połączonej. Schemat takie wdrożenie może wyglądać jak:
  
![Schemat nierównej wdrożenia SAP HANA bez połączenia lokacja lokacja](media/hana-vm-operations/hana-simple-networking2.PNG)
 


## <a name="operations"></a>Operacje
### <a name="backup-and-restore-operations-on-azure-vms"></a>Kopia zapasowa i przywracanie operacje na maszynach wirtualnych Azure
W tych dokumentach opisano możliwości SAP HANA tworzenia kopii zapasowej i przywracania:

- [Omówienie tworzenia kopii zapasowej SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [Kopia zapasowa oprogramowania SAP HANA na poziomie plików](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Testu porównawczego migawki SAP HANA magazynu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)



### <a name="start-and-restart-of-vms-containing-sap-hana"></a>Uruchom i ponowne uruchomienie maszyn wirtualnych zawierających SAP HANA
Jednym z sile chmurze publicznej systemu Azure jest fakt, że są naliczane tylko wtedy minut obliczeniowe, które są wydatków. Oznacza to, że wyłączenie maszyny Wirtualnej z SAP HANA uruchomione w nim tylko koszty przechowywania są rozliczane w tym czasie. Uruchom maszynę Wirtualną z SAP HANA w niej ponownie, maszyna wirtualna będzie mogła się uruchomić ponownie i będzie mieć tych samych adresów IP (jeśli została wdrożona przy użyciu statycznych adresów IP). 


### <a name="saprouter-enabling-sap-remote-support"></a>Włączanie obsługi zdalnego SAPRouter SAP
Jeśli masz połączenie lokacja lokacja między lokalizacje w sieci lokalnej i platformy Azure i uruchomieniem już składniki SAP, istnieje duże prawdopodobieństwo, że już uruchamiasz SAProuter już. W takim przypadku nie ma nic potrzebnych do wykonania SAP HANA wystąpienia, które można wdrożyć na platformie Azure. Z wyjątkiem Obsługa obsługującego HANA w konfiguracji SAPRouter prywatne i statyczny adres IP maszyny Wirtualnej i mieć NSG obsługi podsieci maszyny Wirtualnej HANA dostosowane (ruch przez port TCP/IP portu 3299 dozwolone).

Jeśli wdrażasz SAP HANA i połączyć na platformie Azure za pośrednictwem Internetu, nie masz Router SAP zainstalowane w sieci wirtualnej z systemem Maszynę wirtualną z SAP HANA SAPRouter należy zainstalować w osobnych maszyn wirtualnych w podsieci zarządzania, jak pokazano poniżej :


![Schemat nierównej wdrożenia SAP HANA bez połączenia lokacja lokacja i SAPRouter](media/hana-vm-operations/hana-simple-networking3.PNG)

Należy zainstalować SAPRouter w oddzielnych maszyny Wirtualnej, a nie w Jumpbox maszyny Wirtualnej. Oddzielne maszyna wirtualna musi mieć statyczny adres IP. Aby można było nawiązać połączenia z SAPRouter SAPRouter obsługującego SAP (odpowiednik wystąpienia SAPRouter zainstalować maszyny Wirtualnej), musisz skontaktować się z programu SAP, aby uzyskać adres IP z SAP, które należy skonfigurować wystąpienie SAPRouter. Port tylko niezbędne jest TCP port 3299.
Aby uzyskać więcej informacji na temat sposobu Konfiguracja i konserwacja zdalnego obsługują połączenia za pośrednictwem SAPRouter Sprawdź [źródła SAP](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Wysokiej dostępności z SAP HANA na maszynach wirtualnych Azure natywnego
SUSE Linux 12 z dodatkiem SP1 i nowszej można utworzyć klastra rozrusznik z urządzeniami STONITH, aby ustawić konfigurację SAP HANA, korzystającego z replikacji synchronicznej replikacji systemu HANA i automatycznej pracy awaryjnej. Procedura instalacji została opisana w artykule [dostępności wysokiej programu SAP HANA na maszynach wirtualnych Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability).

 










