---
title: "Typy adresów IP na platformie Azure | Microsoft Docs"
description: "Dowiedz się więcej na temat publicznych i prywatnych adresów IP na platformie Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-resource-manager
ms.assetid: 610b911c-f358-4cfe-ad82-8b61b87c3b7e
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 3de0b167d0ad32de17093caf7e66a6d08f5c1c61
ms.openlocfilehash: 762b048056752abd24328433ceb57de492dbf884


---
# <a name="ip-address-types-and-allocation-methods-in-azure"></a>Typy adresów IP i metody alokacji na platformie Azure
Do zasobów platformy Azure można przypisać adresy IP w celu komunikowania się z innymi zasobami platformy Azure, siecią lokalną i Internetem. Istnieją dwa typy adresów IP, których można użyć na platformie Azure:

* **Publiczne adresy IP**: używane do komunikacji z Internetem łącznie z publicznymi usługami platformy Azure
* **Prywatne adresy IP**: używane do komunikacji w ramach sieci wirtualnej platformy Azure i Twojej lokalnej sieci, gdy używasz bramy sieci VPN lub obwodu ExpressRoute w celu rozszerzenia swojej sieci na platformę Azure.

> [!NOTE]
> Platforma Azure oferuje dwa różne modele wdrażania związane z tworzeniem zasobów i pracą z nimi: [model wdrażania przy użyciu usługi Azure Resource Manager i model klasyczny](../resource-manager-deployment-model.md).  Ten artykuł dotyczy używania modelu wdrażania usługi Resource Manager zalecanego przez firmę Microsoft w przypadku większości nowych wdrożeń zamiast [klasycznego modelu wdrażania](virtual-network-ip-addresses-overview-classic.md).
> 

Jeśli znasz klasyczny model wdrażania, sprawdź [różnice w adresowaniu IP między wersją klasyczną i usługą Resource Manager](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Publiczne adresy IP
Publiczne adresy IP umożliwiają zasobom platformy Azure komunikację z Internetem i publicznymi usługami Azure, takimi jak [Azure Redis Cache](https://azure.microsoft.com/services/cache/), [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) [SQL Database](../sql-database/sql-database-technical-overview.md) i [Azure Storage](../storage/storage-introduction.md).

W usłudze Azure Resource Manager [publiczny adres IP](resource-groups-networking.md#public-ip-address) jest zasobem, który ma własne właściwości. Zasób publicznego adresu IP możesz powiązać z dowolnym z poniższych zasobów:

* Maszyny wirtualne (VM)
* Moduły równoważenia obciążenia dostępne z Internetu
* Bramy sieci VPN
* Bramy aplikacji

### <a name="allocation-method"></a>Metoda alokacji
Istnieją dwie metody, w których adres IP jest przydzielany do *publicznych* zasobów IP — *dynamiczna* lub *statyczna*. Domyślną metodą alokacji jest metoda *dynamiczna*, w której adres IP **nie** jest przydzielany w czasie jego tworzenia. Zamiast tego publiczny adres IP jest przydzielany, gdy uruchamiasz (lub tworzysz) skojarzony zasób (np. maszynę wirtualną lub moduł równoważenia obciążenia). Adres IP jest zwalniany, gdy zatrzymasz (lub usuniesz) zasób. Powoduje to, że adres IP zmienia się, gdy zatrzymujesz i uruchamiasz zasób.

Aby zapewnić, żeby adres IP skojarzonego zasobu pozostawał taki sam, należy ustawić metodę alokacji jawnie jako *statyczną*. W takim przypadku adres IP jest przypisywany natychmiast. Jest on zwalniany tylko wtedy, gdy usuniesz zasób lub zmienisz jego metodę alokacji na *dynamiczną*.

> [!NOTE]
> Nawet gdy ustawisz metodę alokacji jako *statyczną*, nie możesz określić rzeczywistego adresu IP przypisanego do *publicznego zasobu adresu IP*. Zamiast tego jest on przydzielany z puli adresów IP dostępnych w lokalizacji Azure, w której zasób jest tworzony.
>

Statyczne publiczne adresy IP są powszechnie używane w następujących scenariuszach:

* Użytkownicy końcowi muszą aktualizować reguły zapory, aby komunikować się z Twoimi zasobami platformy Azure.
* Rozpoznawanie nazw DNS, gdzie zmiana adresu IP wymaga aktualizacji rekordów A.
* Zasoby platformy Azure komunikują się z innymi aplikacjami lub usługami, które korzystają z modelu zabezpieczeń opartych na adres IP.
* Używasz certyfikatów SSL połączonych z adresem IP.

> [!NOTE]
> Lista zakresów IP, z której publiczne adresy IP (dynamiczne/statyczne) są przydzielane do zasobów platformy Azure, jest opublikowana w [zakresach adresów IP centrum danych platformy Azure](https://www.microsoft.com/download/details.aspx?id=41653).
>

### <a name="dns-hostname-resolution"></a>Rozpoznawanie nazw hostów DNS
Możesz określić etykietę nazwy domeny DNS dla publicznego zasobu adresu IP, który tworzy mapowanie witryny *domainnamelabel*.*location*.cloudapp.azure.com na publiczny adres IP na serwerach DNS zarządzanych przez platformę Azure. Jeśli na przykład utworzysz publiczny zasób adresu IP mający **contoso** jako wartość parametru *domainnamelabel* w *lokalizacji* platformy Azure w **Zachodnich stanach USA**, w pełni kwalifikowana nazwa domeny (FQDN) **contoso.westus.cloudapp.azure.com** zostanie rozpoznana jako publiczny adres IP zasobu. Tej nazwy FQDN możesz użyć do utworzenia rekordu CNAME domeny niestandardowej wskazującego na publiczny adres IP na platformie Azure.

> [!IMPORTANT]
> Każda utworzona etykieta nazwy domeny musi być unikatowa w swojej lokalizacji na platformie Azure.  
>

### <a name="virtual-machines"></a>Maszyny wirtualne
Możesz skojarzyć publiczny adres IP z maszyną wirtualną systemu [Windows](../virtual-machines/virtual-machines-windows-about.md) lub [Linux](../virtual-machines/virtual-machines-linux-about.md), przypisując go do **interfejsu sieciowego**. W przypadku maszyny wirtualnej mającej wiele interfejsów sieciowych możesz przypisać go tylko do *głównego* interfejsu sieciowego. Do maszyny wirtualnej możesz przypisać dynamiczny lub statyczny publiczny adres IP.

### <a name="internet-facing-load-balancers"></a>Moduły równoważenia obciążenia dostępne z Internetu
Publiczny adres IP możesz skojarzyć z usługą [Azure Load Balancer](../load-balancer/load-balancer-overview.md), przypisując go do konfiguracji **frontonu** modułu równoważenia obciążenia. Ten publiczny adres IP służy jako wirtualny adres IP (VIP) o zrównoważonym obciążeniu. Do frontonu modułu równoważenia obciążenia możesz przypisać dynamiczny lub statyczny publiczny adres IP. Do frontonu modułu równoważenia obciążenia możesz także przypisać wiele publicznych adresów IP, co umożliwiają scenariusze [wielu VIP](../load-balancer/load-balancer-multivip.md), takie jak środowisko wielodostępne z witrynami sieci Web opartymi na protokole SSL.

### <a name="vpn-gateways"></a>Bramy sieci VPN
[Brama sieci VPN platformy Azure](../vpn-gateway/vpn-gateway-about-vpngateways.md) służy do łączenia sieci wirtualnej platformy Azure z innymi sieciami wirtualnymi platformy Azure lub siecią lokalną. Musisz przypisać publiczny adres IP jego **konfiguracji IP**, aby mógł on komunikować się z siecią zdalną. Obecnie do bramy sieci VPN możesz przypisać tylko *dynamiczny* publiczny adres IP.

### <a name="application-gateways"></a>Bramy aplikacji
Publiczny adres IP możesz skojarzyć z usługą [Application Gateway](../application-gateway/application-gateway-introduction.md) platformy Azure, przypisując go do konfiguracji **frontonu** bramy. Ten publiczny adres IP służy jako adres VIP o zrównoważonym obciążeniu. Obecnie do konfiguracji frontonu bramy aplikacji możesz przypisać tylko *dynamiczny* publiczny adres IP.

### <a name="at-a-glance"></a>W skrócie
W poniższej tabeli przedstawiono określone właściwości, za pomocą których publiczny adres IP można skojarzyć z zasobem najwyższego poziomu, oraz ewentualne metody alokacji (dynamicznej lub statycznej), których można użyć.

| Zasób najwyższego poziomu | Skojarzenie adresu IP | Dynamiczny | Statyczny |
| --- | --- | --- | --- |
| Maszyna wirtualna |Interfejs sieciowy |Tak |Tak |
| Moduł równoważenia obciążenia |Konfiguracja frontonu |Tak |Tak |
| Brama sieci VPN |Konfiguracja adresu IP bramy |Tak |Nie |
| Brama aplikacji |Konfiguracja frontonu |Tak |Nie |

## <a name="private-ip-addresses"></a>Prywatne adresy IP
Prywatne adresy IP umożliwiają zasobom platformy Azure komunikację z innymi zasobami w [sieci wirtualnej](virtual-networks-overview.md) lub sieci lokalnej za pomocą bramy sieci VPN lub obwodu ExpressRoute bez użycia adresu IP dostępnego w Internecie.

W modelu wdrażania przy użyciu usługi Azure Resource Manager prywatny adres IP jest skojarzony z następującymi typami zasobów platformy Azure:

* Maszyny wirtualne
* Wewnętrzne moduły równoważenia obciążenia (ILB)
* Bramy aplikacji

### <a name="allocation-method"></a>Metoda alokacji
Prywatny adres IP jest przydzielany z zakresu adresów w podsieci, do której zasób jest dołączony. Zakres adresów samej podsieci jest częścią zakresu adresów sieci wirtualnej.

Istnieją dwie metody przydzielania prywatnego adresu IP: *dynamiczna* i *statyczna*. Domyślną metodą alokacji jest metoda *dynamiczna*, gdzie adres IP jest przydzielany automatycznie z podsieci zasobu (przy użyciu protokołu DHCP). Ten adres IP może się zmieniać, gdy zatrzymujesz i uruchamiasz zasób.

Możesz ustawić metodę alokacji jako *statyczną*, aby zapewnić, że adres IP pozostaje bez zmian. W takim przypadku musisz również podać prawidłowy adres IP, który jest częścią podsieci zasobu.

Statyczne prywatne adresy IP są powszechnie używane do:

* Maszyn wirtualnych, które działają jako kontrolery domeny lub serwery DNS.
* Zasobów, które wymagają reguł zapory korzystających z adresów IP.
* Zasoby dostępne dla innych aplikacji/zasobów za pomocą adresu IP.

### <a name="virtual-machines"></a>Maszyny wirtualne
Prywatny adres IP jest przypisywany do **interfejsu sieciowego** maszyny wirtualnej z systemem [Windows](../virtual-machines/virtual-machines-windows-about.md) lub [Linux](../virtual-machines/virtual-machines-linux-about.md). W przypadku maszyny wirtualnej z wieloma interfejsami sieciowymi każdy interfejs otrzymuje przypisany prywatny adres IP. Metodę alokacji dla interfejsu sieciowego możesz określić jako dynamiczną lub statyczną.

#### <a name="internal-dns-hostname-resolution-for-vms"></a>Wewnętrzne rozpoznawanie nazwy hosta DNS (dla maszyn wirtualnych)
Wszystkie maszyny wirtualne platformy Azure są domyślnie skonfigurowane z [serwerami DNS zarządzanymi przez platformę Azure](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution), chyba że jawnie skonfigurujesz niestandardowe serwery DNS. Te serwery DNS zapewniają rozpoznawanie nazw wewnętrznych maszyn wirtualnych znajdujących się w tej samej sieci wirtualnej.

Podczas tworzenia maszyny wirtualnej mapowanie nazwy hosta na jego prywatny adres IP jest dodawane do serwerów DNS zarządzanych przez platformę Azure. W przypadku maszyny wirtualnej z wieloma interfejsami sieciowymi nazwa hosta jest mapowana na prywatny adres IP głównego interfejsu sieciowego.

Maszyny wirtualne skonfigurowane przy użyciu serwerów DNS zarządzanych przez platformę Azure będą mogły rozpoznać nazwy hostów wszystkich maszyn wirtualnych w ramach ich sieci wirtualnej według ich prywatnych adresów IP.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Wewnętrzne moduły równoważenia obciążenia (ILB) i bramy aplikacji
Prywatny adres IP możesz przypisać do konfiguracji **frontonu** [wewnętrznego modułu równoważenia obciążenia platformy Azure](../load-balancer/load-balancer-internal-overview.md) (ILB) lub [bramy aplikacji platformy Azure](../application-gateway/application-gateway-introduction.md). Ten prywatny adres IP służy jako wewnętrzny punkt końcowy dostępny tylko dla zasobów w ramach jego sieci wirtualnej i sieci zdalnych podłączonych do sieci wirtualnej. Do konfiguracji frontonu możesz przypisać dynamiczny albo statyczny prywatny adres IP.

### <a name="at-a-glance"></a>W skrócie
W poniższej tabeli przedstawiono określone właściwości, za pomocą których prywatny adres IP można skojarzyć z zasobem najwyższego poziomu, oraz ewentualne metody alokacji (dynamiczną lub statyczną), których można użyć.

| Zasób najwyższego poziomu | Skojarzenie adresu IP | Dynamiczny | Statyczny |
| --- | --- | --- | --- |
| Maszyna wirtualna |Interfejs sieciowy |Tak |Tak |
| Moduł równoważenia obciążenia |Konfiguracja frontonu |Tak |Tak |
| Brama aplikacji |Konfiguracja frontonu |Tak |Tak |

## <a name="limits"></a>Limity
Ograniczenia nakładane na adresowanie IP zostały wymienione w pełnym zestawieniu [ograniczeń dla sieci](../azure-subscription-service-limits.md#networking-limits) na platformie Azure. Te ograniczenia są podzielone według regionu i subskrypcji. [Kontaktując się z pomocą techniczną](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade), możesz zwiększyć domyślne limity do maksimum w zależności od potrzeb biznesowych.

## <a name="pricing"></a>Cennik
Publiczne adresy IP mogą być związane z nominalnymi opłatami. Aby dowiedzieć się więcej o cenach adresów IP na platformie Azure, przejrzyj stronę [Cennik adresów IP](https://azure.microsoft.com/pricing/details/ip-addresses).

## <a name="next-steps"></a>Następne kroki
* [Wdrażanie maszyny wirtualnej ze statycznym publicznym adresem IP przy użyciu witryny Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
* [Wdrażanie maszyny wirtualnej ze statycznym publicznym adresem IP przy użyciu szablonu](virtual-network-deploy-static-pip-arm-template.md)
* [Wdrażanie maszyny wirtualnej ze statycznym prywatnym adresem IP przy użyciu witryny Azure Portal](virtual-networks-static-private-ip-arm-pportal.md)



<!--HONumber=Jan17_HO5-->


