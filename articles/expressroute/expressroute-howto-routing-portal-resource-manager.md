<properties
   pageTitle="Jak skonfigurować routing dla obwodu usługi ExpressRoute za pośrednictwem portalu Azure | Microsoft Azure"
   description="Ten artykuł zawiera instrukcje tworzenia i inicjowania obsługi komunikacji równorzędnej prywatnej, publicznej i firmy Microsoft obwodu usługi ExpressRoute. W tym artykule opisano również, jak aktualizować i usuwać komunikację równoległą dla obwodu oraz sprawdzać jej stan."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="hero-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/08/2016"
   ms.author="cherylmc"/>

# Tworzenie i modyfikowanie routingu dla obwodu usługi ExpressRoute



> [AZURE.SELECTOR]
[Azure Portal — model usługi Resource Manager](expressroute-howto-routing-portal-resource-manager.md)
[Program PowerShell — model usługi Resource Manager](expressroute-howto-routing-arm.md)
[PowerShell — model klasyczny](expressroute-howto-routing-classic.md)



Ten artykuł zawiera instrukcje tworzenia konfiguracji routingu i zarządzania nią dla obwodu usługi ExpressRoute przy użyciu portalu Azure i modelu wdrażania usługi Resource Manager.

**Modele wdrażania Azure — informacje**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## Wymagania wstępne dotyczące konfiguracji

- Pamiętaj, aby przed rozpoczęciem konfiguracji przejrzeć strony z [wymaganiami wstępnymi](expressroute-prerequisites.md), [wymaganiami routingu](expressroute-routing.md) oraz [przepływami pracy](expressroute-workflows.md).
- Musisz mieć aktywny obwód usługi ExpressRoute. Zanim przejdziesz dalej, postępuj zgodnie z instrukcjami, aby [utworzyć obwód usługi ExpressRoute](expressroute-howto-circuit-arm.md), który powinien zostać włączony przez dostawcę połączenia. Obwód usługi ExpressRoute musi być zainicjowany i włączony, aby można było uruchamiać polecenia cmdlet opisane poniżej.

Te instrukcje dotyczą tylko obwodów utworzonych przy pomocy dostawców oferujących usługi łączności warstwy 2. Jeśli korzystasz z usług dostawcy oferującego zarządzane usługi warstwy 3 (zwykle IPVPN, np. MPLS), dostawca połączenia skonfiguruje routing i będzie nim zarządzać. 


>[AZURE.IMPORTANT] Obecnie nie anonsujemy komunikacji równorzędnej skonfigurowanej przez dostawców usług w portalu zarządzania usługami. Pracujemy nad tym, by wkrótce włączyć tę funkcję. Sprawdź, czy oferuje ją dostawca usług, przed skonfigurowaniem komunikacji równorzędnej BGP.

Można skonfigurować jedną komunikację równorzędną, dwie lub trzy (prywatną Azure, publiczną Azure i Microsoft) dla obwodu usługi ExpressRoute. Możesz skonfigurować komunikację równorzędną w dowolnej kolejności. Musisz jednak pamiętać, aby kończyć konfiguracje poszczególnych komunikacji równorzędnych pojedynczo. 

## Prywatna komunikacja równorzędna Azure

Ta sekcja zawiera instrukcje dotyczące tworzenia, pobierania, aktualizowania i usuwania konfiguracji prywatnej komunikacji równorzędnej Azure dla obwodu usługi ExpressRoute. 

### Aby utworzyć prywatną komunikację równorzędną

1. Skonfiguruj obwód usługi ExpressRoute. Zanim przejdziesz dalej, upewnij się, że obwód jest w całości obsługiwany przez dostawcę połączenia.

    ![](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)


2. Skonfiguruj prywatną komunikację równorzędną Azure dla obwodu. Zanim przejdziesz do następnych kroków, upewnij się, czy masz następujące elementy:

    - Podsieć /30 dla połączenia podstawowego. Nie może ona być częścią żadnej przestrzeni adresowej zarezerwowanej dla sieci wirtualnych.
    - Podsieć /30 dla połączenia dodatkowego. Nie może ona być częścią żadnej przestrzeni adresowej zarezerwowanej dla sieci wirtualnych.
    - Prawidłowy identyfikator sieci VLAN do ustanowienia tej komunikacji równorzędnej jest włączony. Upewnij się, że żadna inna komunikacja równorzędna w obwodzie nie używa tego samego identyfikatora VLAN.
    - Numer AS do komunikacji równorzędnej. Możesz używać 2-bajtowych i 4-bajtowych numerów AS. Możesz użyć prywatnego numeru AS dla tej komunikacji równorzędnej. Pamiętaj, aby nie używać numeru 65515.
    - Skrót MD5, jeśli zdecydujesz się go użyć. **Jest to opcjonalne**.


3. Wybierz wiersz dotyczący prywatnej komunikacji równorzędnej Azure, jak pokazano poniżej.
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rprivate1.png)
    

4. Skonfiguruj prywatną komunikację równorzędną. Obraz poniżej przedstawia przykładową konfigurację.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

    
5. Po określeniu wszystkich parametrów zapisz konfigurację. Po pomyślnym zaakceptowaniu konfiguracji zostanie wyświetlony ekran przypominający przykład poniżej.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)
    

### Aby wyświetlić szczegóły dotyczące prywatnej komunikacji równorzędnej Azure

Możesz wyświetlić właściwości prywatnej komunikacji równorzędnej Azure, wybierając ją.

![](./media/expressroute-howto-routing-portal-resource-manager/rprivate3.png)


### Aby zaktualizować konfigurację prywatnej komunikacji równorzędnej Azure

Można wybrać wiersz dotyczący komunikacji równorzędnej i zmodyfikować jej właściwości. 

![](./media/expressroute-howto-routing-portal-resource-manager/rprivate2.png)

### Aby usunąć prywatną komunikację równorzędną Azure

Możesz usunąć konfigurację komunikacji równorzędnej, wybierając ikonę usuwania, jak pokazano poniżej.

![](./media/expressroute-howto-routing-portal-resource-manager/rprivate4.png)


## Publiczna komunikacja równorzędna Azure

Ta sekcja zawiera instrukcje dotyczące tworzenia, pobierania, aktualizowania i usuwania konfiguracji publicznej komunikacji równorzędnej Azure dla obwodu usługi ExpressRoute. 

### Aby utworzyć publiczną komunikację równorzędną Azure

1. Skonfiguruj obwód usługi ExpressRoute. Zanim przejdziesz dalej, upewnij się, że obwód jest w całości obsługiwany przez dostawcę połączenia.

    ![](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)


2. Skonfiguruj publiczną konfigurację równorzędną Azure dla obwodu. Zanim przejdziesz do następnych kroków, upewnij się, czy masz następujące elementy:

    - Podsieć /30 dla połączenia podstawowego. 
    - Podsieć /30 dla połączenia dodatkowego. 
    - Wszystkie adresy IP użyte do konfiguracji tej komunikacji równorzędnej muszą być prawidłowymi publicznymi adresami IPv4.
    - Prawidłowy identyfikator sieci VLAN do ustanowienia tej komunikacji równorzędnej jest włączony. Upewnij się, że żadna inna komunikacja równorzędna w obwodzie nie używa tego samego identyfikatora VLAN.
    - Numer AS do komunikacji równorzędnej. Możesz używać 2-bajtowych i 4-bajtowych numerów AS.
    - Skrót MD5, jeśli zdecydujesz się go użyć. **Jest to opcjonalne**.

3. Wybierz wiersz dotyczący publicznej komunikacji równorzędnej Azure, jak pokazano poniżej.
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rpublic1.png)
    

4. Skonfiguruj publiczną komunikację równorzędną. Obraz poniżej przedstawia przykładową konfigurację.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

    
5. Po określeniu wszystkich parametrów zapisz konfigurację. Po pomyślnym zaakceptowaniu konfiguracji zostanie wyświetlony ekran przypominający przykład poniżej.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)
    

### Aby wyświetlić szczegóły dotyczące publicznej komunikacji równorzędnej Azure

Możesz wyświetlić właściwości publicznej komunikacji równorzędnej Azure, wybierając ją.

![](./media/expressroute-howto-routing-portal-resource-manager/rpublic3.png)


### Aby zaktualizować konfigurację publicznej komunikacji równorzędnej Azure

Można wybrać wiersz dotyczący komunikacji równorzędnej i zmodyfikować jej właściwości. 

![](./media/expressroute-howto-routing-portal-resource-manager/rpublic2.png)

### Aby usunąć publiczną komunikację równorzędną Azure

Możesz usunąć konfigurację komunikacji równorzędnej, wybierając ikonę usuwania, jak pokazano poniżej.

![](./media/expressroute-howto-routing-portal-resource-manager/rpublic4.png)


## Komunikacja równorzędna firmy Microsoft

Ta sekcja zawiera instrukcje dotyczące tworzenia, pobierania, aktualizowania i usuwania konfiguracji komunikacji równorzędnej Microsoft dla obwodu usługi ExpressRoute. 

### Aby utworzyć komunikację równorzędną Microsoft

1. Skonfiguruj obwód usługi ExpressRoute. Zanim przejdziesz dalej, upewnij się, że obwód jest w całości obsługiwany przez dostawcę połączenia.

    ![](./media/expressroute-howto-routing-portal-resource-manager/listprovisioned.png)


2. Skonfiguruj komunikację równorzędną Microsoft dla obwodu. Zanim przejdziesz dalej, upewnij się, że masz poniższe informacje.

    - Podsieć /30 dla połączenia podstawowego. Musi to być prawidłowy publiczny prefiks IPv4, którego jesteś właścicielem, zarejestrowany w RIR/IRR.
    - Podsieć /30 dla połączenia dodatkowego. Musi to być prawidłowy publiczny prefiks IPv4, którego jesteś właścicielem, zarejestrowany w RIR/IRR.
    - Prawidłowy identyfikator sieci VLAN do ustanowienia tej komunikacji równorzędnej jest włączony. Upewnij się, że żadna inna komunikacja równorzędna w obwodzie nie używa tego samego identyfikatora VLAN.
    - Numer AS do komunikacji równorzędnej. Możesz używać 2-bajtowych i 4-bajtowych numerów AS.
    - **Anonsowane prefiksy:** musisz podać listę wszystkich prefiksów, które planujesz anonsować za pośrednictwem sesji BGP. Akceptowane są tylko prefiksy publicznych adresów IP. Jeśli zamierzasz wysłać zestaw prefiksów, możesz wysłać listę oddzielaną przecinkami. Prefiksy te muszą być zarejestrowane na Ciebie w RIR/IRR.
    - **Numer ASN klienta:** jeśli anonsujesz prefiksy, które nie są rejestrowane do numeru AS komunikacji równorzędnej, możesz określić numer AS, do którego są rejestrowane. **Jest to opcjonalne**.
    - **Nazwa rejestru routingu:** możesz określić RIR/IRR, względem którego rejestrowany jest numer AS i prefiksy. **Jest to opcjonalne.**
    - Skrót MD5, jeśli zdecydujesz się go użyć. **Jest to opcjonalne.**
    
3. Możesz wybrać komunikację równorzędną, którą chcesz skonfigurować, jak pokazano poniżej. Zaznacz wiersz dotyczący komunikacji równorzędnej firmy Microsoft.
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft1.png)
    

4.  Skonfiguruj komunikację równorzędną firmy Microsoft. Obraz poniżej przedstawia przykładową konfigurację.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft2.png)

    
5. Po określeniu wszystkich parametrów zapisz konfigurację. 

    Jeśli obwód przejdzie w stan wymagający sprawdzenia poprawności (jak pokazano poniżej), musisz otworzyć bilet pomocy technicznej, aby okazać dowód własności prefiksów zespołowi pomocy technicznej.  
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft5.png)


    Bilet pomocy technicznej możesz otworzyć bezpośrednio w portalu, jak pokazano poniżej.   
    
    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft6.png)


6. Po pomyślnym zaakceptowaniu konfiguracji zostanie wyświetlony ekran przypominający przykład poniżej.

    ![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)
    

### Aby wyświetlić szczegóły dotyczące komunikacji równorzędnej firmy Microsoft

Możesz wyświetlić właściwości publicznej komunikacji równorzędnej Azure, wybierając ją.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft3.png)


### Aby zaktualizować konfigurację komunikacji równorzędnej firmy Microsoft

Można wybrać wiersz dotyczący komunikacji równorzędnej i zmodyfikować jej właściwości. 


![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft7.png)


### Aby usunąć komunikację równorzędną firmy Microsoft

Możesz usunąć konfigurację komunikacji równorzędnej, wybierając ikonę usuwania, jak pokazano poniżej.

![](./media/expressroute-howto-routing-portal-resource-manager/rmicrosoft4.png)



## Następne kroki

Następny krok: [Link a VNet to an ExpressRoute circuit](expressroute-howto-linkvnet-arm.md) (Łączenie sieci wirtualnej z obwodem usługi ExpressRoute).

-  Więcej informacji na temat przepływów pracy usługi ExpressRoute znajduje się w artykule [ExpressRoute workflows](expressroute-workflows.md) (Przepływy pracy usługi ExpressRoute).

-  Aby uzyskać więcej informacji o komunikacji równorzędnej obwodu, zobacz artykuł [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (Obwody i domeny routingu usługi ExpressRoute).

-  Więcej informacji na temat pracy z sieciami wirtualnymi znajduje się w artykule [Virtual network overview](../virtual-network/virtual-networks-overview.md) (Omówienie sieci wirtualnych).




<!--HONumber=jun16_HO2-->


