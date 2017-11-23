---
title: "Konfigurowanie zapory aplikacji sieci Web (WAF) do środowiska usługi aplikacji"
description: "Informacje o sposobie konfigurowania zapory aplikacji sieci web przed środowiska usługi aplikacji."
services: app-service\web
documentationcenter: 
author: naziml
manager: erikre
editor: jimbe
ms.assetid: a2101291-83ba-4169-98a2-2c0ed9a65e8d
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: naziml
ms.openlocfilehash: 3c218a6fe3857c216bc185c5d3630025f332147b
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/23/2017
---
# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Konfigurowanie zapory aplikacji sieci Web (WAF) do środowiska usługi aplikacji
## <a name="overview"></a>Omówienie
Zapory aplikacji sieci Web, takich jak [Barracuda zapory aplikacji sieci Web dla platformy Azure](https://www.barracuda.com/programs/azure) dostępnych na [portalu Azure Marketplace](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) ułatwia zabezpieczenie aplikacji sieci web, sprawdzając ruch przychodzący sieci web do blokowania przekazywania złośliwego oprogramowania iniekcji skryptów krzyżowych, SQL i aplikacji przed atakami DDoS i inne ataki. Sprawdza również odpowiedzi z serwerów sieci web zaplecza dla zapobiegania utracie danych (DLP). W połączeniu z izolacji i dodatkowe skalowanie podał środowiska usługi App Service, zapewnia to idealne środowiska do hosta biznesowe krytyczne aplikacje sieci web muszą wytrzymać złośliwymi żądaniami i dużych obciążeń ruchem.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Konfiguracja
Dla tego dokumentu możemy skonfigurować środowisko usługi aplikacji za wiele wystąpień ze zrównoważonym obciążeniem Barracuda zapory aplikacji sieci Web tak, aby tylko ruch z zapory aplikacji sieci Web można osiągnąć środowiska usługi aplikacji i nie jest on dostępny od strefy DMZ. Mamy także usługi Azure Traffic Manager przed wystąpień Barracuda zapory aplikacji sieci Web w celu zrównoważenia obciążenia w centrach danych platformy Azure i regionów. Diagramu wysokiego poziomu ustawień będzie wyglądać poniższej ilustracji:

![Architektura][Architecture] 

> [!NOTE]
> Wraz z wprowadzeniem [ILB obsługę środowiska usługi aplikacji](app-service-environment-with-internal-load-balancer.md), można skonfigurować ASE niedostępny od strefy DMZ i udostępniane wyłącznie do sieci prywatnej. 
> 
> 

## <a name="configuring-your-app-service-environment"></a>Konfigurowanie środowiska usługi aplikacji
Aby skonfigurować środowisko usługi aplikacji, zapoznaj się [naszej dokumentacji](app-service-web-how-to-create-an-app-service-environment.md) na ten temat. Po utworzeniu środowiska usługi aplikacji utworzony, można utworzyć aplikacji sieci Web, aplikacje interfejsu API i [Mobile Apps](../../app-service-mobile/app-service-mobile-value-prop.md) w tym środowisku, które będą wszystkie chronione za skonfigurowanie w następnej sekcji zapory aplikacji sieci Web.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Konfigurowanie usługi w chmurze zapory aplikacji sieci Web Barracuda
Ma barracuda [szczegółowe artykułu](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) na temat wdrażania jego zapory aplikacji sieci Web na maszynie wirtualnej na platformie Azure. Jednak ponieważ firma Microsoft ma nadmiarowości i wprowadza pojedynczy punkt awarii, którą chcesz wdrożyć co najmniej dwie maszyny wirtualne wystąpienie zapory aplikacji sieci Web do tej samej usługi w chmurze, gdy następujące instrukcje.

### <a name="adding-endpoints-to-cloud-service"></a>Dodawanie punktów końcowych do usługi w chmurze
Gdy masz 2 lub wirtualna zapory aplikacji sieci Web więcej wystąpień w usłudze w chmurze, możesz użyć [portalu Azure](https://portal.azure.com/) można dodać punktów końcowych HTTP i HTTPS, które są używane przez aplikację, jak pokazano na poniższej ilustracji:

![Konfigurowanie punktu końcowego][ConfigureEndpoint]

Aplikacji korzystanie z innych punktów końcowych, należy je dodać do tej listy, a także. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Konfigurowanie Barracuda zapory aplikacji sieci Web za pomocą portalu zarządzania
Barracuda zapory aplikacji sieci Web używa TCP Port 8000 dla konfiguracji za pomocą portalu zarządzania. Jeśli masz wiele wystąpień maszyn wirtualnych zapory aplikacji sieci Web, należy powtórzyć kroki opisane w tym miejscu dla każdego wystąpienia maszyny Wirtualnej. 

> [!NOTE]
> Po zakończeniu konfiguracji zapory aplikacji sieci Web, należy usunąć punkt końcowy protokołu TCP/8000 z Twojej VMs zapory aplikacji sieci Web w celu poprawy bezpieczeństwa Twojej zapory aplikacji sieci Web.
> 
> 

Dodaj punkt końcowy zarządzania, jak pokazano na poniższej ilustracji skonfigurować Twoje Barracuda zapory aplikacji sieci Web.

![Dodaj punkt końcowy zarządzania][AddManagementEndpoint]

Korzystanie z przeglądarki, aby przejść do zarządzania punktem końcowym usługi w chmurze. Usługi w chmurze jest nazywany test.cloudapp.net, czy dostęp do tego punktu końcowego przechodząc do http://test.cloudapp.net:8000. Powinna zostać wyświetlona strona logowania, takie jak poniższy obraz, który możesz zalogować się przy użyciu poświadczeń, których określono w fazie konfiguracji maszyny Wirtualnej zapory aplikacji sieci Web.

![Zarządzanie strony logowania][ManagementLoginPage]

Po zalogowaniu powinien zostać wyświetlony pulpit nawigacyjny tak jak na poniższej ilustracji, który przedstawia podstawowe dane statystyczne dotyczące ochrony zapory aplikacji sieci Web.

![Pulpit nawigacyjny zarządzania][ManagementDashboard]

Kliknięcie **usług** kartę umożliwia skonfigurowanie zapory Twojej aplikacji sieci Web dla usługi ochrony. Aby uzyskać więcej informacji na temat konfigurowania sieci Barracuda zapory aplikacji sieci Web, zobacz [dokumentacji](https://techlib.barracuda.com/waf/getstarted1). W poniższym przykładzie skonfigurowano aplikacji sieci Web platformy Azure obsługujące ruch HTTP i HTTPS.

![Dodaj zarządzania usługi][ManagementAddServices]

> [!NOTE]
> W zależności od sposobu konfiguracji aplikacji i jakie funkcje są używane w środowisku usługi aplikacji należy przekazywać ruch dla protokołu TCP porty inne niż 80 i 443, na przykład, jeśli masz ustawienia protokołu SSL z adresu IP dla aplikacji sieci Web. Listę porty używane w środowisku usługi aplikacji, zobacz [ruch przychodzący kontroli dokumentacji](app-service-app-service-environment-control-inbound-traffic.md) sekcji porty sieciowe.
> 
> 

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Konfigurowanie Menedżera ruchu Microsoft Azure (OPCJONALNIE)
Jeśli aplikacja jest dostępna w wielu regionach, a następnie chcesz załadować saldo je za [usługi Azure Traffic Manager](../../traffic-manager/traffic-manager-overview.md). Aby to zrobić, można dodać punktu końcowego w [portalu Azure](https://portal.azure.com) przy użyciu nazwy usługi w chmurze dla Twojej zapory aplikacji sieci Web w profilu Menedżera ruchu, jak pokazano na poniższej ilustracji. 

![Punkt końcowy Menedżera ruchu][TrafficManagerEndpoint]

Jeśli aplikacja wymaga uwierzytelniania, upewnij się, że jakiś zasób, który nie wymaga uwierzytelniania dla Menedżera ruchu sieciowego na polecenie ping dostępność aplikacji. Można skonfigurować adres URL na **konfiguracji** strony [portalu Azure](https://portal.azure.com) jak pokazano na poniższej ilustracji:

![Konfigurowanie usługi Traffic Manager][ConfigureTrafficManager]

Aby przesłać dalej polecenia ping Menedżera ruchu z Twojej zapory aplikacji sieci Web do aplikacji, należy zdefiniować tłumaczenia witryny sieci Web na Twojej Barracuda zapory aplikacji sieci Web, aby przesyłał dalej ruch do aplikacji, jak pokazano w poniższym przykładzie:

![Tłumaczenie witryny sieci Web][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Zabezpieczanie ruchu do środowiska usługi aplikacji przy użyciu grup zabezpieczeń sieci (NSG)
Postępuj zgodnie z [ruch przychodzący kontroli dokumentacji](app-service-app-service-environment-control-inbound-traffic.md) szczegółowe informacje na temat ograniczania ruchu do środowiska usługi aplikacji z zapory aplikacji sieci Web tylko przy użyciu adresu VIP usługi w chmurze. Oto przykład polecenia programu Powershell do wykonywania tego zadania TCP port 80.

    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

Zamień wirtualny adres IP (VIP) usługi w chmurze Twoje WAF SourceAddressPrefix.

> [!NOTE]
> Adres VIP usługi w chmurze zostanie zmieniona, gdy usunięcie i ponowne utworzenie usługi w chmurze. Upewnij się zaktualizować adres IP w tej grupie zasobów sieciowych, wówczas. 
> 
> 

<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
