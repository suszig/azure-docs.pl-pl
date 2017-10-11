---
title: "Konfigurowanie sieci wirtualnej i bramy w przypadku połączeń ExpressRoute w klasycznym portalu | Dokumentacja firmy Microsoft"
description: "Ten artykuł przeprowadzi Cię przez proces konfigurowania sieci wirtualnej dla usługi przy użyciu klasycznego modelu wdrażania i klasycznego portalu."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: 688817d9-51c8-4372-9af8-33fa098c7c5a
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/20/2016
ms.author: cherylmc
ms.openlocfilehash: f62254b2a7df50aa55a2a49009702848a9aecebd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-network-for-expressroute-in-the-classic-portal"></a>Tworzenie sieci wirtualnej dla usługi ExpressRoute w klasycznym portalu
Kroki opisane w tym artykule przeprowadzi Cię przez konfigurację sieci wirtualnej i Brama sieci wirtualnej do użycia z ExpressRoute przy użyciu klasycznego modelu wdrażania i klasycznego portalu.

Jeśli szukasz instrukcje dotyczące modelu wdrażania usługi Resource Manager, można użyć następujących artykułów: [utworzyć sieć wirtualną przy użyciu programu PowerShell](../virtual-network/virtual-networks-create-vnet-arm-ps.md) i [dodać bramę sieci VPN do sieci wirtualnej Menedżera zasobów dla usługi ExpressRoute](expressroute-howto-add-gateway-resource-manager.md).

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**Modele wdrażania Azure — informacje**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="create-a-classic-vnet-and-gateway"></a>Tworzenie klasycznej sieci wirtualnej i bramy
Następujące kroki tworzenia klasycznych sieci wirtualnej i Brama sieci wirtualnej. Jeśli masz już klasycznej sieci wirtualnej, zobacz [skonfigurować istniejącej sieci wirtualnej klasycznego](#config) w tym artykule.

1. Zaloguj się do [klasycznej witryny Azure Portal](http://manage.windowsazure.com).
2. W lewym dolnym rogu ekranu kliknij **nowy**. W okienku nawigacji kliknij opcję **Usługi sieciowe**, a następnie opcję **Sieć wirtualna**. Kliknij przycisk **Tworzenie niestandardowe**, aby rozpocząć korzystanie z kreatora konfiguracji.
3. Na **szczegóły sieci wirtualnych** strony, wprowadź następujące:
   
   * **Nazwa** — nazwa sieci wirtualnej. Podczas wdrażania swoich wystąpień maszyn wirtualnych i PaaS, dlatego nie może być można utworzyć nazwy zbyt skomplikowane, będziesz używać tej nazwy sieci wirtualnej.
   * **Lokalizacja** — lokalizacja jest bezpośrednio powiązana do fizycznej lokalizacji (regionu) miejscu (VM) do znajdują się zasoby. Jeśli na przykład maszyny wirtualne wdrażane w ramach tej sieci wirtualnej mają znajdować się we wschodnim regionie USA, należy wybrać tę właśnie lokalizację. Nie można zmienić regionu skojarzonego z sieci wirtualnej, po jego utworzeniu.
4. Na stronie **DNS Servers and VPN Connectivity** (Serwery DNS i połączenia sieci VPN) wprowadź następujące informacje, a następnie kliknij strzałkę w prawym dolnym rogu, aby przejść dalej. 
   
   * **Serwery DNS** — wprowadź nazwę serwera DNS i adres IP lub wybierz wcześniej zarejestrowanego serwera DNS z menu skrótów. To ustawienie nie powoduje jednak utworzenia serwera DNS. Umożliwia natomiast określenie serwerów DNS, które mają być używane do rozpoznawania nazw dla tej sieci wirtualnej.
   * **Połączenie lokacja-lokacja** -zaznacz pole wyboru **skonfigurowania sieci VPN lokacja lokacja**.
   * **ExpressRoute** — pole wyboru **ExpressRoute użyj**. Ta opcja jest dostępna tylko, jeśli wybrano **skonfigurowania sieci VPN lokacja-lokacja**.
   * **Sieci lokalnej** — są wymagane, aby witryna sieci lokalnej w przypadku połączeń ExpressRoute. Jednak w przypadku połączenia ExpressRoute, prefiksy adresów określonych dla lokacji sieci lokalnej zostanie zignorowany. Zamiast tego prefiksy adresów anonsowane do firmy Microsoft za pośrednictwem obwodu ExpressRoute będzie służyć do celów routingu.<BR>Jeśli masz już sieć lokalną utworzone dla połączenia ExpressRoute, zostanie ona wybrana z listy rozwijanej. Jeśli nie, wybierz **Określ sieć lokalną**.
5. **Połączenie lokacja-lokacja** strona jest wyświetlana, jeśli został wybrany do określenia sieci lokalnej w poprzednim kroku. Aby skonfigurować sieci lokalnej, wprowadź następujące informacje, a następnie kliknij strzałkę dalej. 
   
   * **Nazwa** — lokacji sieciowej nazwę, która ma wywołać lokalne (lokalnego).
   * **Przestrzeń adresowa** — w tym początkowy adres IP i CIDR (liczba adresów). Można określić żadnych zakres adresów, tak długo, jak jego nie pokrywają się z zakresem adresów dla sieci wirtualnej. Zazwyczaj będzie to określ zakresy adresów dla sieci lokalnej, ale w przypadku usługi ExpressRoute, te ustawienia nie są używane. Jednak to ustawienie jest wymagane w celu utworzenia sieci lokalnej, korzystając z klasycznego portalu.
   * **Dodawanie przestrzeni adresów** — to ustawienie nie jest ważna w przypadku połączeń ExpressRoute.
6. Na **przestrzeniami adresów sieci wirtualnej** strony, wprowadź następujące informacje, a następnie kliknij znacznik wyboru w prawej dolnej do konfiguracji sieci. 
   
   * **Przestrzeń adresowa** — tym uruchamianie IP i adres count. Upewnij się, że przestrzeni adresowych, które określisz nie nakłada przestrzeni adresowych, które mają w sieci lokalnej.
   * **Dodaj podsieć** — w tym uruchamianie adresów IP i liczba adresów. Dodatkowe podsieci nie są wymagane.
   * **Dodaj podsieć bramy** — kliknij, aby dodać podsieci bramy. Podsieć bramy jest używana tylko dla bramy sieci wirtualnej i jest wymagana dla tej konfiguracji.<BR>Podsieć bramy CIDR (liczba adresów) w przypadku połączeń ExpressRoute musi być /28 lub większy (/ 27, / 26 itp.). Umożliwia to za mało adresów IP w tej podsieci umożliwia konfigurację do pracy. W klasycznym portalu Jeśli zaznaczono pole wyboru, aby używać usługi ExpressRoute, portalu określa z /28 podsieci bramy.  Nie można dostosować liczba adresów CIDR w klasycznym portalu. Podsieć bramy będzie wyświetlany jako **bramy** w klasycznym portalu, mimo że rzeczywista nazwa podsieci bramy, która jest tworzona jest rzeczywiście **GatewaySubnet**. Ta nazwa można wyświetlić przy użyciu programu PowerShell lub w portalu Azure.
7. Kliknij znacznik wyboru na dole strony. Spowoduje to rozpoczęcie procesu tworzenia sieci wirtualnej. Po zakończeniu wykonywania, zobaczysz **utworzony** kategorii **stan** na **sieci** strony w klasycznym portalu.

## <a name="gw"></a>Tworzenie bramy
1. Na **sieci** strony, kliknij przycisk sieci wirtualnej, który został właśnie utworzony, a następnie kliknij przycisk **pulpitu nawigacyjnego** w górnej części strony.
2. W dolnej części **pulpitu nawigacyjnego** kliknij przycisk **Tworzenie bramy** i wybierz **routingu dynamicznego**. Kliknij przycisk **tak** aby upewnić się, że chcesz utworzyć bramę.
3. Po uruchomieniu bramy tworzenie zobaczysz komunikat, dzięki czemu będzie wiadomo, że brama została uruchomiona. Może potrwać do 45 minut bramy do utworzenia.
4. Połącz z siecią obwodu. Postępuj zgodnie z instrukcjami w artykule [jak połączyć sieci wirtualnych z obwody usługi ExpressRoute](expressroute-howto-linkvnet-classic.md).

## <a name="config"></a>Konfigurowanie klasycznego istniejącej sieci wirtualnej dla usługi ExpressRoute
Jeśli masz już klasycznej sieci wirtualnej, można skonfigurować go do nawiązania połączenia ExpressRoute w klasycznym portalu. Ustawienia są w taki sam jak powyżej, więc przeczytaj za pośrednictwem tych sekcji, aby zapoznać się z wymaganymi ustawieniami. Jeśli chcesz utworzyć połączenie coexisting ExpressRoute/lokacja-lokacja, zobacz [w tym artykule](expressroute-howto-coexist-classic.md) instrukcje. Są one różne od kroki opisane w tym artykule.

1. Należy utworzyć sieci lokalnej, przed uaktualnieniem reszty ustawień sieci wirtualnej. Aby utworzyć nowy sieciach lokalnych, co jest wymagane podczas konfigurowania usługi ExpressRoute za pośrednictwem klasycznego portalu, kliknij przycisk **nowy**  **>**  **usług sieciowych**  **>**  **sieci wirtualnej**  **>**  **sieci lokalnej Dodaj**. Wykonaj kroki kreatora, aby utworzyć sieci lokalnej.
2. Użyj **Konfiguruj** strony, aby zaktualizować pozostałe ustawienia sieci wirtualnej i skojarzyć sieć wirtualną do sieci lokalnej.
3. Po skonfigurowaniu ustawień, przejdź do [utworzenia bramy](#gw) sekcji tego artykułu, aby utworzyć bramę.

## <a name="next-steps"></a>Następne kroki
* Jeśli chcesz dodać maszyn wirtualnych do sieci wirtualnej, zobacz [ścieżki szkoleniowe dotyczące maszyn wirtualnych](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/).
* Jeśli chcesz dowiedzieć się więcej na temat połączenia ExpressRoute, zobacz [omówienie ExpressRoute](expressroute-introduction.md).

