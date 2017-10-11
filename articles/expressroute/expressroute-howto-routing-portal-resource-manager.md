---
title: "Jak skonfigurować obwód (równorzędna) dla usługi routingu: Menedżer zasobów: Azure | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera instrukcje tworzenia i inicjowania obsługi komunikacji równorzędnej prywatnej, publicznej i firmy Microsoft obwodu usługi ExpressRoute. W tym artykule opisano również, jak aktualizować i usuwać komunikację równoległą dla obwodu oraz sprawdzać jej stan."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8c2a7ed2-ae5c-4e49-81f6-77cf9f2b2ac9
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: cherylmc
ms.openlocfilehash: 55ccadfea55b8098ee58dcaef942f6ba54093665
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a>Tworzenie i modyfikowanie komunikacji równorzędnej dla obwodu usługi ExpressRoute

W tym artykule opisano tworzenie i zarządzanie nimi konfiguracji routingu dla obwodu usługi ExpressRoute w modelu wdrażania usługi Resource Manager przy użyciu portalu Azure. Można również sprawdzić stan, update lub delete i anulowanie zastrzeżenia komunikacji równorzędnych dla obwodu usługi ExpressRoute. Jeśli chcesz użyć innej metody do pracy z obwodu, wybierz artykułu z poniższej listy:


## <a name="configuration-prerequisites"></a>Wymagania wstępne dotyczące konfiguracji

* Pamiętaj, aby przed rozpoczęciem konfiguracji przejrzeć strony z [wymaganiami wstępnymi](expressroute-prerequisites.md), [wymaganiami routingu](expressroute-routing.md) oraz [przepływami pracy](expressroute-workflows.md).
* Musisz mieć aktywny obwód usługi ExpressRoute. Zanim przejdziesz dalej, postępuj zgodnie z instrukcjami, aby [utworzyć obwód usługi ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md), który powinien zostać włączony przez dostawcę połączenia. Obwód usługi expressroute musi być w stanie zainicjowane i włączone dla możesz mieć możliwość uruchamiania poleceń cmdlet w kolejnych sekcjach.
* Jeśli planujesz używać udostępnionego skrótu MD5/klucz, należy użyć tej funkcji po obu stronach tunelu i ograniczyć liczbę znaków do maksymalnie 25.

Te instrukcje dotyczą tylko obwodów utworzonych przy pomocy dostawców oferujących usługi łączności warstwy 2. Jeśli używasz usługodawcy, który oferuje zarządzanych warstwy 3 usługi (zazwyczaj IPVPN, takich jak MPLS), dostawca połączenia konfiguruje i zarządza nimi routingu dla Ciebie. 

> [!IMPORTANT]
> Obecnie nie anonsujemy komunikacji równorzędnej skonfigurowanej przez dostawców usług w portalu zarządzania usługami. Pracujemy nad tym, by wkrótce włączyć tę funkcję. Skontaktuj się z dostawcą usług przed rozpoczęciem konfigurowania komunikacji równorzędnych protokołu BGP.
> 
> 

Można skonfigurować jedną komunikację równorzędną, dwie lub trzy (prywatną Azure, publiczną Azure i Microsoft) dla obwodu usługi ExpressRoute. Możesz skonfigurować komunikację równorzędną w dowolnej kolejności. Musisz jednak pamiętać, aby kończyć konfiguracje poszczególnych komunikacji równorzędnych pojedynczo.

## <a name="azure-private-peering"></a>Prywatna komunikacja równorzędna Azure

Ta sekcja pomoże Ci tworzenie, get, aktualizowanie i usuwanie konfiguracji platformy Azure prywatnej komunikacji równorzędnej dla obwodu usługi ExpressRoute.

### <a name="to-create-azure-private-peering"></a>Aby utworzyć prywatną komunikację równorzędną

1. Skonfiguruj obwód usługi ExpressRoute. Zanim przejdziesz dalej, upewnij się, że obwód jest w całości obsługiwany przez dostawcę połączenia.

  ![Lista](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Skonfiguruj prywatną komunikację równorzędną Azure dla obwodu. Zanim przejdziesz do następnych kroków, upewnij się, czy masz następujące elementy:

  * Podsieć /30 dla połączenia podstawowego. Podsieci nie może być częścią żadnych przestrzeni adresowej zarezerwowane dla sieci wirtualnych.
  * Podsieć /30 dla połączenia dodatkowego. Podsieci nie może być częścią żadnych przestrzeni adresowej zarezerwowane dla sieci wirtualnych.
  * Prawidłowy identyfikator sieci VLAN do ustanowienia tej komunikacji równorzędnej jest włączony. Upewnij się, że żadna inna komunikacja równorzędna w obwodzie nie używa tego samego identyfikatora VLAN.
  * Numer AS do komunikacji równorzędnej. Możesz używać 2-bajtowych i 4-bajtowych numerów AS. Możesz użyć prywatnego numeru AS dla tej komunikacji równorzędnej. Pamiętaj, aby nie używać numeru 65515.
  * **Opcjonalne -** skrótu MD5, jeśli chcesz użyć jednego.
3. Wybierz Azure prywatnej komunikacji równorzędnej wierszy, jak pokazano w poniższym przykładzie:

  ![Prywatne](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
4. Skonfiguruj prywatną komunikację równorzędną. Na poniższej ilustracji przedstawiono przykład konfiguracji:

  ![Skonfiguruj prywatnej komunikacji równorzędnej](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)
5. Po określeniu wszystkich parametrów zapisz konfigurację. Po zaakceptowaniu pomyślnie konfiguracji, zobacz podobny do poniższego przykładu:

  ![Zapisz prywatnej komunikacji równorzędnej](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="to-view-azure-private-peering-details"></a>Aby wyświetlić szczegóły dotyczące prywatnej komunikacji równorzędnej Azure

Możesz wyświetlić właściwości prywatnej komunikacji równorzędnej Azure, wybierając ją.

![Wyświetl prywatnej komunikacji równorzędnej](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)

### <a name="to-update-azure-private-peering-configuration"></a>Aby zaktualizować konfigurację prywatnej komunikacji równorzędnej Azure

Można wybrać wiersz dotyczący komunikacji równorzędnej i zmodyfikować jej właściwości.

![Zaktualizuj prywatnej komunikacji równorzędnej](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### <a name="to-delete-azure-private-peering"></a>Aby usunąć prywatną komunikację równorzędną Azure

Można usunąć konfiguracji komunikacji równorzędnej, wybierając ikonę Usuń, jak pokazano na poniższej ilustracji:

![usunięcie prywatnej komunikacji równorzędnej](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)

## <a name="azure-public-peering"></a>Publiczna komunikacja równorzędna Azure

Ta sekcja pomoże Ci tworzenie, get, aktualizowanie i usuwanie konfiguracji platformy Azure publicznej komunikacji równorzędnej dla obwodu usługi ExpressRoute.

### <a name="to-create-azure-public-peering"></a>Aby utworzyć publiczną komunikację równorzędną Azure

1. Skonfiguruj obwód usługi ExpressRoute. Zanim przejdziesz dalej, upewnij się, że obwód jest w całości obsługiwany przez dostawcę połączenia.

  ![Lista publicznej komunikacji równorzędnej](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Skonfiguruj publiczną konfigurację równorzędną Azure dla obwodu. Zanim przejdziesz do następnych kroków, upewnij się, czy masz następujące elementy:

  * Podsieć /30 dla połączenia podstawowego. Musi to być prawidłowy publiczny prefiks IPv4.
  * Podsieć /30 dla połączenia dodatkowego. Musi to być prawidłowy publiczny prefiks IPv4.
  * Prawidłowy identyfikator sieci VLAN do ustanowienia tej komunikacji równorzędnej jest włączony. Upewnij się, że żadna inna komunikacja równorzędna w obwodzie nie używa tego samego identyfikatora VLAN.
  * Numer AS do komunikacji równorzędnej. Możesz używać 2-bajtowych i 4-bajtowych numerów AS.
  * **Opcjonalne -** skrótu MD5, jeśli chcesz użyć jednego.
3. Wybierz Azure wiersza publicznej komunikacji równorzędnej, jak pokazano na poniższej ilustracji:

  ![Wybierz publicznej komunikacji równorzędnej wiersza](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
4. Skonfiguruj publiczną komunikację równorzędną. Na poniższej ilustracji przedstawiono przykład konfiguracji:

  ![Skonfiguruj publicznej komunikacji równorzędnej](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)
5. Po określeniu wszystkich parametrów zapisz konfigurację. Po zaakceptowaniu pomyślnie konfiguracji, zobacz podobny do poniższego przykładu:

  ![Zapisz publicznej komunikacji równorzędnej konfiguracji](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="to-view-azure-public-peering-details"></a>Aby wyświetlić szczegóły dotyczące publicznej komunikacji równorzędnej Azure

Możesz wyświetlić właściwości publicznej komunikacji równorzędnej Azure, wybierając ją.

![właściwości publicznej komunikacji równorzędnej widoku](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)

### <a name="to-update-azure-public-peering-configuration"></a>Aby zaktualizować konfigurację publicznej komunikacji równorzędnej Azure

Można wybrać wiersz dotyczący komunikacji równorzędnej i zmodyfikować jej właściwości.

![Wybierz publicznej komunikacji równorzędnej wiersza](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### <a name="to-delete-azure-public-peering"></a>Aby usunąć publiczną komunikację równorzędną Azure

Można usunąć konfiguracji komunikacji równorzędnej, wybierając ikonę Usuń, jak pokazano w poniższym przykładzie:

![Usuń publicznej komunikacji równorzędnej](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)

## <a name="microsoft-peering"></a>Komunikacja równorzędna firmy Microsoft

Ta sekcja pomoże Ci tworzenie, get, aktualizowanie i usuwanie konfiguracji komunikacji równorzędnej firmy Microsoft dla obwodu usługi ExpressRoute.

> [!IMPORTANT]
> Z obwody usługi ExpressRoute, które zostały skonfigurowane przed 1 sierpnia 2017 komunikacji równorzędnej firmy Microsoft ma wszystkie prefiksy usługi anonsowane przez firmę Microsoft, zaglądanie, nawet jeśli nie zdefiniowano filtrów trasy. Z obwody usługi ExpressRoute, które są skonfigurowane na lub po 1 sierpnia 2017 komunikacji równorzędnej firmy Microsoft nie będzie miał wszystkie prefiksy anonsowane do momentu filtr tras jest dołączony do obwodu. Aby uzyskać więcej informacji, zobacz [skonfigurować filtr trasy dla komunikacji równorzędnej firmy Microsoft](how-to-routefilter-powershell.md).
> 
> 

### <a name="to-create-microsoft-peering"></a>Aby utworzyć komunikację równorzędną Microsoft

1. Skonfiguruj obwód usługi ExpressRoute. Zanim przejdziesz dalej, upewnij się, że obwód jest w całości obsługiwany przez dostawcę połączenia.

  ![Lista komunikacji równorzędnej firmy Microsoft](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)
2. Skonfiguruj komunikację równorzędną Microsoft dla obwodu. Zanim przejdziesz dalej, upewnij się, że masz poniższe informacje.

  * Podsieć /30 dla połączenia podstawowego. Musi to być prawidłowy publiczny prefiks IPv4, którego jesteś właścicielem, zarejestrowany w RIR/IRR.
  * Podsieć /30 dla połączenia dodatkowego. Musi to być prawidłowy publiczny prefiks IPv4, którego jesteś właścicielem, zarejestrowany w RIR/IRR.
  * Prawidłowy identyfikator sieci VLAN do ustanowienia tej komunikacji równorzędnej jest włączony. Upewnij się, że żadna inna komunikacja równorzędna w obwodzie nie używa tego samego identyfikatora VLAN.
  * Numer AS do komunikacji równorzędnej. Możesz używać 2-bajtowych i 4-bajtowych numerów AS.
  * Anonsowane prefiksy: musisz podać listę wszystkich prefiksów, które planujesz anonsować za pośrednictwem sesji BGP. Akceptowane są tylko prefiksy publicznych adresów IP. Jeśli planujesz wysłać zestaw prefiksy, możesz wysłać rozdzielana przecinkami lista. Prefiksy te muszą być zarejestrowane na Ciebie w RIR/IRR.
  * **Opcjonalne -** klienta ASN: jeśli prefiksy reklamy, które nie zostały zarejestrowane do komunikacji równorzędnej jako numer, można określić numer AS, z którym są rejestrowane.
  * Nazwa rejestru routingu: możesz określić RIR/IRR, względem którego rejestrowany jest numer AS i prefiksy.
  * **Opcjonalne -** skrótu MD5, jeśli chcesz użyć jednego.
3. Możesz wybrać komunikacji równorzędnej, którą chcesz skonfigurować, jak pokazano w poniższym przykładzie. Zaznacz wiersz dotyczący komunikacji równorzędnej firmy Microsoft.

  ![Wybierz wiersz komunikacji równorzędnej firmy Microsoft](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
4. Skonfiguruj komunikację równorzędną firmy Microsoft. Na poniższej ilustracji przedstawiono przykład konfiguracji:

  ![Konfigurowanie komunikacji równorzędnej firmy Microsoft](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)
5. Po określeniu wszystkich parametrów zapisz konfigurację.

  Jeśli obwodu pobiera do "Weryfikacji potrzebne" stan (jak pokazano w obrazie), należy otworzyć bilet pomocy technicznej, aby przedstawić dowód własności prefiksów do działu pomocy technicznej.

  ![Zapisywanie konfiguracji komunikacji równorzędnej firmy Microsoft](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)

  Możesz otworzyć bilet pomocy technicznej bezpośrednio z portalu, jak pokazano w poniższym przykładzie:

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


1. Po zaakceptowaniu pomyślnie konfiguracji, zobacz podobną do poniższej ilustracji:

  ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="to-view-microsoft-peering-details"></a>Aby wyświetlić szczegóły dotyczące komunikacji równorzędnej firmy Microsoft

Możesz wyświetlić właściwości publicznej komunikacji równorzędnej Azure, wybierając ją.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)

### <a name="to-update-microsoft-peering-configuration"></a>Aby zaktualizować konfigurację komunikacji równorzędnej firmy Microsoft

Można wybrać wiersz dotyczący komunikacji równorzędnej i zmodyfikować jej właściwości.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)

### <a name="to-delete-microsoft-peering"></a>Aby usunąć komunikację równorzędną firmy Microsoft

Można usunąć konfiguracji komunikacji równorzędnej, wybierając ikonę Usuń, jak pokazano na poniższej ilustracji:

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)

## <a name="next-steps"></a>Następne kroki

Następny krok [połączyć sieć wirtualną z obwodem usługi ExpressRoute](expressroute-howto-linkvnet-portal-resource-manager.md)
* Więcej informacji na temat przepływów pracy usługi ExpressRoute znajduje się w artykule [ExpressRoute workflows](expressroute-workflows.md) (Przepływy pracy usługi ExpressRoute).
* Aby uzyskać więcej informacji o komunikacji równorzędnej obwodu, zobacz artykuł [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (Obwody i domeny routingu usługi ExpressRoute).
* Więcej informacji na temat pracy z sieciami wirtualnymi znajduje się w artykule [Virtual network overview](../virtual-network/virtual-networks-overview.md) (Omówienie sieci wirtualnych).
