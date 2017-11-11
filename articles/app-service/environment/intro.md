---
title: "Wprowadzenie do środowiska usługi aplikacji Azure"
description: "Krótki przegląd środowiska usługi aplikacji Azure"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 3c7eaefa-1850-4643-8540-428e8982b7cb
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 2a619943abaf8835e591872cba0ed046d4c6c4a9
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2017
---
# <a name="introduction-to-app-service-environments"></a>Wprowadzenie do środowiska usługi aplikacji #
 
## <a name="overview"></a>Omówienie ##

Środowiska usługi aplikacji Azure to funkcja usługi Azure App Service, która udostępnia środowisko pełni izolowanym środowisku, aby bezpiecznie pracować aplikacji usługi App Service na dużą skalę. Ta funkcja może obsługiwać aplikacje sieci web [aplikacje mobilne][mobileapps], aplikacje interfejsu API i [funkcje][Functions].

Środowiska usługi aplikacji (ASEs) są odpowiednie dla obciążeń aplikacji, które wymagają:

- Bardzo dużej skali.
- Izolacja i bezpiecznego dostępu do sieci.
- Użycie pamięci wysokiej.

Klienci mogą tworzyć wiele ASEs w pojedynczym regionie Azure lub w wielu regionach platformy Azure. Tego rodzaju elastyczności sprawia, że ASEs nadaje się doskonale dla warstwy aplikacji bezstanowych, w związku z wysoką obciążeniami RPS skalowanie w poziomie.

ASEs izolowane do uruchamiania tylko jednego odbiorcy aplikacji i zawsze są wdrażane w sieci wirtualnej. Klienci mają precyzyjną kontrolę nad aplikacji dla ruchu przychodzącego i wychodzącego ruchu sieciowego. Aplikacje mogą nawiązywać bezpiecznych połączeń o dużej szybkości za pośrednictwem sieci VPN do zasobów firmy lokalnie.

* ASEs włączyć hosting aplikacji wysokiej skali z bezpiecznego dostępu do sieci. Aby uzyskać więcej informacji, zobacz [nowości AzureCon](https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/) na ASEs.
* Wiele ASEs umożliwia skalowanie w poziomie. Aby uzyskać więcej informacji, zobacz [sposobu konfigurowania wpływ aplikacja rozproszona geograficznie](app-service-app-service-environment-geo-distributed-scale.md).
* ASEs może służyć do konfigurowania Architektura zabezpieczeń, jak pokazano w AzureCon nowości. Aby dowiedzieć się, jak pokazano nowości AzureCon architektury zabezpieczeń został skonfigurowany, zobacz [artykuł na temat sposobu implementacja architektury zabezpieczeń warstwowych](app-service-app-service-environment-layered-security.md) ze środowiska usługi aplikacji.
* Aplikacje działające na ASEs może mieć ich dostęp uzyskiwany za nadrzędnego urządzeń, takich jak zapory aplikacji sieci web (WAFs). Aby uzyskać więcej informacji, zobacz [Konfigurowanie zapory aplikacji sieci Web dla środowiska usługi aplikacji](app-service-app-service-environment-web-application-firewall.md).

## <a name="dedicated-environment"></a>Dedykowanym środowisku ##

ASE jest przeznaczona wyłącznie do jednej subskrypcji i może zawierać 100 wystąpień. Zakres może obejmować 100 wystąpień w jeden plan usługi aplikacji do 100 planów usługi aplikacji w jednym wystąpieniu, a wszystko w pomiędzy.

ASE składa się z interfejsy i pracowników. Interfejsy są zobowiązani do zakończenia połączenia HTTP/HTTPS i automatyczne równoważenie obciążenia żądań aplikacji w elemencie ASE. Interfejsy są automatycznie dodawane jako planów usługi App Service w ASE jest skalowana w poziomie.

Pracownicy są role, które hostowanie aplikacji klienta. Pracownicy są dostępne w trzech rozmiarach stałe:

* Jeden vCPU/3.5 GB pamięci RAM
* Dwa vCPU/7 GB pamięci RAM
* 4 vCPU/14 GB pamięci RAM

Klienci nie muszą zarządzać interfejsy i pracowników. Wszystkie infrastruktury jest automatycznie dodawane jako klienci skalowania planów usługi aplikacji. Zgodnie z planami usługi aplikacji są tworzone lub skalowany w elemencie ASE, że wymagana infrastruktura dodaniu lub usunięciu odpowiednio.

Brak płaskim miesięczne szybkości dla ASE, który pokrywa infrastruktury i nie zmienia się rozmiar ASE. Ponadto jest koszt vCPU planu usługi aplikacji. Wszystkie aplikacje obsługiwane w elemencie ASE znajdują się w izolowany cennik jednostki SKU. Aby uzyskać informacje o cenach dla ASE, zobacz [cennik usługi aplikacji] [ Pricing] strony i przejrzyj dostępne opcje ASEs.

## <a name="virtual-network-support"></a>Obsługa sieci wirtualnej ##

ASE można tworzyć tylko w sieci wirtualnej platformy Azure Resource Manager. Aby dowiedzieć się więcej o sieciach wirtualnych platformy Azure, zobacz [często zadawane pytania dotyczące sieci wirtualnej platformy Azure](https://azure.microsoft.com/documentation/articles/virtual-networks-faq/). ASE zawsze istnieje w sieci wirtualnej, a dokładnie w podsieci sieci wirtualnej. Funkcje zabezpieczeń sieci wirtualnych służy do kontroli ruchu przychodzącego i wychodzącego komunikacji dla aplikacji.

ASE może być skierowane do Internetu za pomocą publicznego adresu IP lub wewnętrzny uwzględniającym tylko adres (ILB) usługi równoważenia obciążenia wewnętrznego platformy Azure.

[Sieciowe grupy zabezpieczeń] [ NSGs] ograniczenia komunikacji sieciowej dla ruchu przychodzącego do podsieci, w której znajduje się ASE. Grupy NSG służy do uruchamiania aplikacji związany z nadrzędnego urządzeń i usług, takich jak WAFs i dostawców SaaS sieci.

Aplikacje muszą również często dostępu do zasobów firmy, takich jak wewnętrznej bazy danych i usług sieci web. Jeśli wdrożono ASE w sieci wirtualnej, który ma połączenie VPN do sieci lokalnej, aplikacje w ASE dostęp do zasobów lokalnych. Ta funkcja ma wartość true, niezależnie od tego, czy sieć VPN [lokacja lokacja](https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/) lub [Azure ExpressRoute](http://azure.microsoft.com/services/expressroute/) sieci VPN.

Aby uzyskać więcej informacji na temat działania ASEs z sieciami wirtualnymi i sieciach lokalnych, zobacz [zagadnienia dotyczące sieci środowiska usługi aplikacji][ASENetwork].

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Application-Service-Environments-v2-Private-PaaS-Environments-in-the-Cloud/player]

## <a name="app-service-environment-v1"></a>Środowisko usługi App Service — wersja 1 ##

Środowiska usługi aplikacji ma dwie wersje: ASEv1 i ASEv2. Oparto na ASEv2 powyższych informacji. W tej sekcji przedstawiono różnice między ASEv1 i ASEv2. 

W ASEv1 trzeba zarządzać wszystkie zasoby ręcznie. Zawierającej interfejsy, pracowników i adresy IP używane dla opartych na protokole SSL. Przed można skalować w poziomie planu usługi aplikacji, konieczne jest pierwszy skalowania puli procesów roboczych, które chcesz udostępnić go.

ASEv1 używa innego modelu cenowego z ASEv2. W ASEv1 płacisz za każdym vCPU przydzielone. Zawierającą Vcpu, interfejsy lub pracowników, którzy nie są hosting dowolnych zadań. W ASEv1 domyślny rozmiar maksymalny skali ASE jest 55 hosty łącznie. Zawierającej pracowników i interfejsy. Jedną z zalet ASEv1 jest, że może on zostać wdrożony w klasycznej sieci wirtualnej i sieci wirtualnych Menedżera zasobów. Aby dowiedzieć się więcej na temat ASEv1, zobacz [wprowadzenie v1 środowiska usługi aplikacji][ASEv1Intro].

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[webapps]: ../app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
