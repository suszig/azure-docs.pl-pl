---
title: "Tworzenie i modyfikowanie obwodu usługi ExpressRoute: portalu Azure | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób tworzenia, obsługi administracyjnej, sprawdź, aktualizacji, usuwania i anulowanie zastrzeżenia obwodu usługi ExpressRoute."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 68d59d59-ed4d-482f-9cbc-534ebb090613
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/07/2017
ms.author: cherylmc;ganesr
ms.openlocfilehash: e3721cd3c031622788f553e71a6555b844f3f7dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-an-expressroute-circuit"></a>Tworzenie i modyfikowanie obwodu usługi ExpressRoute
> [!div class="op_single_selector"]
> * [Witryna Azure Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Interfejs wiersza polecenia platformy Azure](howto-circuit-cli.md)
> * [Video - portalu Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klasyczny)](expressroute-howto-circuit-classic.md)
>

W tym artykule opisano sposób tworzenia obwodu usługi Azure ExpressRoute przy użyciu portalu Azure i modelu wdrażania usługi Azure Resource Manager. Poniższe kroki przedstawiają także sposób Sprawdź stan obwodu, zaktualizować lub usunąć i anulowanie zastrzeżenia go.


## <a name="before-you-begin"></a>Przed rozpoczęciem
* Przegląd [wymagania wstępne](expressroute-prerequisites.md) i [przepływy pracy](expressroute-workflows.md) przed rozpoczęciem konfigurowania.
* Upewnij się, że masz dostęp do [portalu Azure](https://portal.azure.com).
* Upewnij się, że masz uprawnienia do tworzenia nowych zasobów sieciowych. Jeśli nie masz odpowiednich uprawnień, skontaktuj się z administratorem konta.
* Możesz [wyświetlanie wideo](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) przed rozpoczęciem, aby lepiej zrozumieć kroki.

## <a name="create-and-provision-an-expressroute-circuit"></a>Tworzenie i przydzielanie obwodu usługi ExpressRoute
### <a name="1-sign-in-to-the-azure-portal"></a>1. Logowanie się do witryny Azure Portal
Przejdź w przeglądarce do witryny [Azure Portal](http://portal.azure.com) i zaloguj się przy użyciu konta platformy Azure.

### <a name="2-create-a-new-expressroute-circuit"></a>2. Utwórz nowy obwód usługi ExpressRoute
> [!IMPORTANT]
> Od momentu jego wygenerowaniu klucza usługi będą naliczane obwodu usługi ExpressRoute. Upewnij się, gdy dostawca łączności jest gotowy do udostępniania obwodu podczas wykonywania tej operacji.
> 
> 

1. Można utworzyć obwodu usługi ExpressRoute, wybierając opcję, aby utworzyć nowy zasób. Kliknij przycisk **nowy** > **sieci** > **ExpressRoute**, jak pokazano na poniższej ilustracji:
   
    ![Create an ExpressRoute circuit (Tworzenie obwodu usługi ExpressRoute)](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit1.png)
2. Po kliknięciu **ExpressRoute**, zobaczysz **obwodu ExpressRoute utworzyć** bloku. Po jest wypełnianie wartości w tym bloku, upewnij się, że podajesz prawidłowe jednostki SKU warstwy i pomiaru danych.
   
   * **Warstwa** Określa, czy włączone jest standardem ExpressRoute lub dodatek usługi ExpressRoute w warstwie premium. Można określić **standardowe** można pobrać wersji standard lub **Premium** dla dodatku premium.
   * **Pomiaru danych** Określa typ rozliczeń. Można określić **Metered** planu dane naliczane i **nieograniczone** planu nieograniczone danych. Zmiana rozliczeń typu z **Metered** do **bez ograniczeń**, ale nie można zmienić typu z **bez ograniczeń** do **Metered**.
     
     ![Skonfiguruj warstwy jednostki SKU i pomiaru danych](./media/expressroute-howto-circuit-portal-resource-manager/createcircuit2.png)

> [!IMPORTANT]
> Należy pamiętać, że wskazuje lokalizacji elementu równorzędnego [lokalizacji fizycznej](expressroute-locations.md) gdzie są komunikacji równorzędnej z firmą Microsoft. Jest to **nie** połączone z właściwością "Lokalizacja", która odwołuje się do lokalizacji geograficznej, w którym znajduje się dostawcy zasobów sieciowych Azure. Gdy nie są powiązane, jest dobrym rozwiązaniem, aby wybrać dostawcy zasobów sieciowych w lokalizacji geograficznej blisko lokalizacji elementu równorzędnego obwodu. 
> 
> 

### <a name="3-view-the-circuits-and-properties"></a>3. Wyświetl obwody i właściwości
**Wyświetlanie wszystkich obwodów**

Można wyświetlić wszystkich obwodów, które zostały utworzone przez wybranie **wszystkie zasoby** w menu po lewej stronie.

![Obwody widoku](./media/expressroute-howto-circuit-portal-resource-manager/listresource.png)

**Wyświetl właściwości**

    You can view the properties of the circuit by selecting it. On this blade, note the service key for the circuit. You must copy the circuit key for your circuit and pass it down to the service provider to complete the provisioning process. The circuit key is specific to your circuit.

![Wyświetl właściwości](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

### <a name="4-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>4. Wyślij klucz usługi do dostawcą połączenia do inicjowania obsługi
W tym bloku **stan dostawcy** zawiera informacje o bieżącym stanie inicjowania obsługi administracyjnej po stronie dostawcy usług. **Stan obwodu** zapewnia stanu po stronie firmy Microsoft. Aby uzyskać więcej informacji na temat obwodu inicjowania obsługi administracyjnej stanów zobacz [przepływy pracy](expressroute-workflows.md#expressroute-circuit-provisioning-states) artykułu.

Podczas tworzenia nowego obwodu ExpressRoute obwodu będą w następującym stanie:

Stan dostawcy: nie zainicjowano obsługę administracyjną<BR>
Stan obwodu: włączone

![Zainicjuj proces inicjowania obsługi administracyjnej](./media/expressroute-howto-circuit-portal-resource-manager/viewstatus.png)

Obwodu zmieni się na następujący stan, gdy trwa jej włączanie dostawca połączenia:

Stan dostawcy: Inicjowanie obsługi administracyjnej<BR>
Stan obwodu: włączone

Dla Ciebie można było używać obwodu usługi ExpressRoute musi być w następującym stanie:

Stan dostawcy: zainicjowano obsługę administracyjną<BR>
Stan obwodu: włączone

### <a name="5-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>5. Okresowo sprawdzać stan i stan klucz obwodu
Można wyświetlić właściwości obwodu, w którym są zainteresowani przez zaznaczenie go. Sprawdź **stan dostawcy** i upewnij się, że została przeniesiona do **obsługiwane administracyjnie** przed kontynuowaniem.

![Stan obwodu i dostawcy](./media/expressroute-howto-circuit-portal-resource-manager/viewstatusprovisioned.png)

### <a name="6-create-your-routing-configuration"></a>6. Tworzenie konfiguracji routingu
Aby uzyskać instrukcje, zapoznaj się [obwodu ExpressRoute konfiguracji routingu](expressroute-howto-routing-portal-resource-manager.md) artykułu do tworzenia i modyfikowania obwodu komunikacji równorzędnych.

> [!IMPORTANT]
> Te instrukcje dotyczą tylko obwody, które zostały utworzone z dostawców usług, które oferują warstwy 2 łączności usługi. Jeśli używasz usługodawcy, który oferuje zarządzanych warstwy 3 usługi (zazwyczaj IP sieci VPN, takie jak MPLS), dostawca połączenia będzie Konfigurowanie i zarządzanie nimi routingu dla Ciebie.
> 
> 

### <a name="7-link-a-virtual-network-to-an-expressroute-circuit"></a>7. Łączenie sieci wirtualnej z obwodem usługi ExpressRoute
Następnie połączyć sieć wirtualną obwodu usługi ExpressRoute. Użyj [łączenia sieci wirtualne obwody usługi ExpressRoute](expressroute-howto-linkvnet-arm.md) artykuł podczas pracy z modelu wdrażania usługi Resource Manager.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Pobieranie stanu obwodu usługi ExpressRoute
Stan obwodu można wyświetlić, wybierając go. 

![Stan obwodu usługi ExpressRoute](./media/expressroute-howto-circuit-portal-resource-manager/listproperties1.png)

## <a name="modifying-an-expressroute-circuit"></a>Modyfikowanie obwodu usługi ExpressRoute
Można zmodyfikować niektórych właściwości obwodu usługi ExpressRoute, bez wywierania wpływu na łączność.

Można wykonać następujące czynności bez przestojów:

* Włącz lub Wyłącz dodatek premium usługi ExpressRoute dla obwodu usługi ExpressRoute.
* Zwiększyć przepustowość obwodu ExpressRoute dostarczane na port jest dostępna pojemność. Należy pamiętać, że przepustowość obwodu zmiana wersji na starszą nie jest obsługiwany. 
* Zmiana zliczania planu naliczane danych nieograniczone danych. Należy pamiętać, że zmiana zliczania planu z dane nieograniczone naliczane danych nie jest obsługiwana.
* Można włączyć lub wyłączyć *operacje klasycznego*.

Aby uzyskać więcej informacji na limity i ograniczenia dotyczą [ExpressRoute — często zadawane pytania](expressroute-faqs.md).

Aby zmodyfikować obwodu usługi ExpressRoute, wybierz polecenie **konfiguracji** jak pokazano na poniższej ilustracji.

![Modyfikowanie obwodu](./media/expressroute-howto-circuit-portal-resource-manager/modifycircuit.png)

Można modyfikować przepustowości, jednostka SKU, modelu rozliczeń i zezwala na klasycznym operacji w ramach bloku konfiguracji.

> [!IMPORTANT]
> Może być konieczne ponowne utworzenie obwodu usługi expressroute w przypadku niewystarczającego pojemności na istniejącego portu. Nie można uaktualnić obwodu, jeśli nie bez dodatkowej pojemności dostępnej w tej lokalizacji.
>
> Nie można zmniejszyć przepustowość obwodu usługi ExpressRoute bez zakłóceń. Zmiana wersji na starszą przepustowości wymaga anulowanie zastrzeżenia obwodu ExpressRoute, a następnie Udostępnij ponownie nowy obwód usługi ExpressRoute.
> 
> Wyłączenie dodatku premium operacja może zakończyć się niepowodzeniem, jeśli używasz zasobów, które są większe niż co to jest dozwolone dla standardowych obwodu.
> 
> 

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Anulowania obsługi i usuwania obwodu usługi ExpressRoute
Można usunąć obwodu usługi ExpressRoute, wybierając **usunąć** ikony. Pamiętaj o następujących kwestiach:

* Należy odłączyć wszystkie sieci wirtualne z obwodem usługi ExpressRoute. Jeśli ta operacja nie powiedzie się, sprawdź, czy wszystkie sieci wirtualne są połączone z obwodem.
* Jeśli dostawca usługi obwodu ExpressRoute stan inicjowania obsługi jest **inicjowania obsługi administracyjnej** lub **obsługiwane administracyjnie** należy skontaktować się z dostawcą usług na anulowanie zastrzeżenia obwód w bok. Firma Microsoft będzie zarezerwować zasobów i obciążać Cię do czasu dostawcy usług wykonuje anulowania obsługi obwodu i powiadomienia NAS.
* Jeśli usługodawca została anulowana obwodu (ustawiono dostawcę usługi stan inicjowania obsługi **nieudostępniane**) następnie można usunąć obwodu. Spowoduje to zatrzymanie rozliczeń dla obwodu

## <a name="next-steps"></a>Następne kroki
Po utworzeniu obwodu, upewnij się, należy wykonać następujące czynności:

* [Tworzenie i modyfikowanie routingu dla obwodu usługi ExpressRoute](expressroute-howto-routing-portal-resource-manager.md)
* [Link sieci wirtualnej do obwodu usługi ExpressRoute](expressroute-howto-linkvnet-arm.md)

