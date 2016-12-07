---
title: "Tworzenie wirtualnej sieci równorzędnej przy użyciu witryny Azure Portal | Microsoft Docs"
description: "Dowiedz się, jak utworzyć sieć wirtualną przy użyciu portalu Azure w usłudze Resource Manager."
services: virtual-network
documentationcenter: 
author: NarayanAnnamalai
manager: jefco
editor: 
tags: azure-resource-manager
ms.assetid: 026bca75-2946-4c03-b4f6-9f3c5809c69a
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: narayanannamalai;annahar
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 688fed72b32767f33010b9e8f17921b16320072d


---
# <a name="create-a-virtual-network-peering-using-the-azure-portal"></a>Tworzenie wirtualnej sieci równorzędnej przy użyciu portalu Azure
[!INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[!INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Aby utworzyć wirtualną sieć równorzędną w oparciu o powyższy scenariusz, korzystając z portalu Azure, wykonaj poniższe kroki.

1. W przeglądarce przejdź do strony http://portal.azure.com i w razie potrzeby zaloguj się przy użyciu konta platformy Azure.
2. Aby ustanowić komunikację równorzędną w sieci wirtualnej, musisz utworzyć dwa połączenia, jedno dla każdego kierunku, między dwiema sieciami wirtualnymi. Możesz najpierw utworzyć połączenie komunikacji równorzędnej w sieci wirtualnej z sieci VNET1 do sieci VNET2. W portalu kliknij pozycję **Przeglądaj** > **wybierz pozycję Sieci wirtualne**
   
    ![Tworzenie wirtualnej sieci równorzędnej w portalu Azure](./media/virtual-networks-create-vnetpeering-arm-portal/figure01.png)
3. W bloku Sieci wirtualne wybierz sieć VNET1, kliknij pozycję Peerings (Sesje komunikacji równorzędnej), a następnie kliknij przycisk Add (Dodaj).
   
    ![Wybieranie sesji komunikacji równorzędnej](./media/virtual-networks-create-vnetpeering-arm-portal/figure02.png)
4. W bloku Add Peering (Dodaj sesję komunikacji równorzędnej), nadaj połączeniu komunikacji równorzędnej nazwę LinkToVnet2, wybierz subskrypcję oraz równorzędną sieć wirtualną VNET2 i kliknij przycisk OK.
   
    ![Połączenie z siecią wirtualną](./media/virtual-networks-create-vnetpeering-arm-portal/figure03.png)
5. Po utworzeniu tego połączenia wirtualnej sieci równorzędnej wyświetlany będzie następujący stan połączenia:
   
    ![Stan połączenia](./media/virtual-networks-create-vnetpeering-arm-portal/figure04.png)
6. Następnie utwórz połączenie wirtualnej sieci równorzędnej z sieci VNET2 do sieci VNET1. W bloku Sieci wirtualne wybierz sieć VNET2, kliknij pozycję Peerings (Sesje komunikacji równorzędnej), a następnie kliknij przycisk Add (Dodaj).
   
    ![Komunikacja równorzędna z innej sieci wirtualnej](./media/virtual-networks-create-vnetpeering-arm-portal/figure05.png)
7. W bloku Add Peering (Dodaj sesję komunikacji równorzędnej) nadaj połączeniu komunikacji równorzędnej nazwę LinkToVnet1, wybierz subskrypcję oraz równorzędną sieć wirtualną i kliknij przycisk OK.
   
    ![Kafelek tworzenia sieci wirtualnej](./media/virtual-networks-create-vnetpeering-arm-portal/figure06.png)
8. Po utworzeniu tego połączenia wirtualnej sieci równorzędnej wyświetlany będzie następujący stan połączenia:
   
    ![Końcowy stan połączenia](./media/virtual-networks-create-vnetpeering-arm-portal/figure07.png)
9. Należy sprawdzić stan połączenia LinkToVnet2, który powinien również zmienić się na Połączono.  
   
    ![Końcowy stan połączenia 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure08.png)
   
   > [!NOTE]
   > Komunikacja równorzędna w sieci wirtualnej jest ustanowiona tylko wtedy, gdy oba połączenia są połączone.
   > 
   > 

Istnieje kilka właściwości możliwych do skonfigurowania dla każdego połączenia:

| Opcja | Opis | Domyślne |
|:--- |:--- |:--- |
| AllowVirtualNetworkAccess |Określa, czy przestrzeń adresów wirtualnej sieci równorzędnej ma zostać dołączona do tagu Virtual_network. |Tak |
| AllowForwardedTraffic |Określa, czy ruch niepochodzący z równorzędnej sieci wirtualnej jest akceptowany, czy pomijany |Nie |
| AllowGatewayTransit |Umożliwia wirtualnej sieci równorzędnej korzystanie z bramy sieci wirtualnej. |Nie |
| UseRemoteGateways |Określa, czy ma być używana brama wirtualnej sieci równorzędnej. Wirtualna sieć równorzędna musi mieć skonfigurowaną bramę, a właściwość AllowGatewayTransit musi być wybrana. Tej opcji nie można użyć, jeśli w sieci jest skonfigurowana brama. |Nie |

Każde połączenie w wirtualnej sieci równorzędnej ma zestaw właściwości przedstawionych powyżej. W portalu możesz kliknąć połączenie wirtualnej sieci równorzędnej i zmienić dostępne opcje, a następnie kliknąć przycisk Zapisz, aby zastosować zmiany.

[!INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

1. W przeglądarce przejdź do strony http://portal.azure.com i w razie potrzeby zaloguj się przy użyciu konta platformy Azure.
2. W tym przykładzie użyto dwóch subskrypcji, A i B, oraz dwóch użytkowników, UżytkownikA i UżytkownikB, mających uprawnienia w odpowiednich subskrypcjach.
3. W portalu kliknij pozycję Przeglądaj, a następnie wybierz pozycję Sieci wirtualne. Kliknij sieć wirtualną i kliknij przycisk Dodaj.
   
    ![Scenariusz 2 – Przeglądanie](./media/virtual-networks-create-vnetpeering-arm-portal/figure09.png)
4. W bloku Dodawanie uprawnień dostępu kliknij rolę i wybierz pozycję Network Contributor (Współautor sieci), kliknij pozycję Dodaj użytkowników, wpisz nazwę logowania użytkownika B i kliknij przycisk OK.
   
    ![Kontrola dostępu oparta na rolach](./media/virtual-networks-create-vnetpeering-arm-portal/figure10.png)
   
    Nie jest to wymagane, ponieważ komunikację równorzędną można nawiązać, nawet jeśli użytkownicy indywidualnie zgłoszą żądania komunikacji równorzędnej dla odpowiednich sieci wirtualnych, o ile zgłoszenia są zgodne. Dodanie użytkowników o odpowiednich uprawnieniach do innej sieci wirtualnej jako użytkowników w lokalnej sieci wirtualnej ułatwia konfigurowanie za pomocą portalu.
5. Następnie zaloguj się do portalu Azure jako użytkownik B, który jest użytkownikiem o odpowiednich uprawnieniach w subskrypcji B. Wykonaj powyższe kroki, aby dodać użytkownika A jako użytkownika o roli Network Contributor (Współautor sieci).
   
    ![Kontrola dostępu oparta na rolach 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure11.png)
   
   > [!NOTE]
   > Można wylogować i zalogować się w obu sesjach użytkownika w przeglądarce, aby upewnić się, że autoryzacja została włączona pomyślnie.
   > 
   > 
6. Zaloguj się do portalu jako użytkownik A, przejdź do bloku VNET3, kliknij pozycję Peering (Sesja komunikacji równorzędnej), zaznacz pole wyboru „I Know my resource ID” (Znam swój identyfikator zasobu) i wpisz identyfikator zasobu dla sieci VNET5 w formacie przedstawionym poniżej.
   
    /subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.Network/VirtualNetwork/{VNETname}
   
    ![Identyfikator zasobu](./media/virtual-networks-create-vnetpeering-arm-portal/figure12.png)
7. Zaloguj się do portalu jako użytkownik B i wykonaj powyższe czynności, aby utworzyć połączenie z sieci VNET5 do sieci VNET3.
   
    ![Identyfikator zasobu 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure13.png)
8. Komunikacja równorzędna zostanie nawiązana i każda maszyna wirtualna w sieci VNet3 powinna mieć możliwość komunikacji z dowolną maszyną wirtualną w sieci VNet5.

[!INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Pierwszym krokiem będzie utworzenie połączenia komunikacji równorzędnej w sieci wirtualnej z sieci HubVnet do sieci VNET1. Należy zauważyć, że opcja Allow Forwarded Traffic (Zezwalaj na przekazywany ruch) nie jest zaznaczona dla tego połączenia.
   
    ![Podstawowa komunikacja równorzędna](./media/virtual-networks-create-vnetpeering-arm-portal/figure14.png)
2. Kolejnym krokiem będzie utworzenie połączeń komunikacji równorzędnej z sieci VNET1 do sieci HubVnet. Należy zauważyć, że opcja Allow Forwarded Traffic (Zezwalaj na przekazywany ruch) jest zaznaczona.
   
    ![Podstawowa komunikacja równorzędna](./media/virtual-networks-create-vnetpeering-arm-portal/figure15a.png)
3. Po nawiązaniu komunikacji równorzędnej można zapoznać z tym [artykułem](virtual-network-create-udr-arm-ps.md), aby zdefiniować trasę zdefiniowaną przez użytkownika w celu przekierowania ruchu sieci VNet1 za pośrednictwem urządzenia wirtualnego i wykorzystać jego możliwości. Podczas określania adresu następnego skoku w trasie można ustawić go na adres IP urządzenia wirtualnego w wirtualnej sieci równorzędnej HubVNet

[!INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

1. W przeglądarce przejdź do strony http://portal.azure.com i w razie potrzeby zaloguj się przy użyciu konta platformy Azure.
2. Aby nawiązać komunikację równorzędną sieci wirtualnych w ramach tego scenariusza, należy utworzyć tylko jedno łącze z sieci wirtualnej menedżera zasobów platformy Azure do klasycznej sieci wirtualnej. To znaczy z sieci **VNET1** do sieci **VNET2**. W portalu kliknij pozycję **Przeglądaj**, a następnie wybierz pozycję **Sieci wirtualne**.
3. W bloku sieci wirtualnych wybierz pozycję **VNET1**. Kliknij pozycję **Sesje komunikacji równorzędnej**, a następnie kliknij polecenie **Dodaj**.
4. W bloku Dodaj sesję komunikacji równorzędnej wprowadź nazwę łącza. W tym przykładzie jego nazwa to **LinkToVnet2**. W obszarze Szczegóły sieci równorzędnych wybierz opcję **Klasyczny**.
5. Następnie wybierz subskrypcję i równorzędną sieć wirtualną **VNET2**. Następnie kliknij przycisk OK.
   
    ![Łączenie sieci VNET1 z siecią VNET2](./media/virtual-networks-create-vnetpeering-arm-portal/figure18.png)
6. Po utworzeniu łącza komunikacji równorzędnej sieci wirtualnych zostanie nawiązana komunikacja równorzędna między dwiema sieciami wirtualnymi i zostanie wyświetlony następujący stan:
   
    ![Sprawdzanie połączenia komunikacji równorzędnej](./media/virtual-networks-create-vnetpeering-arm-portal/figure19.png)

## <a name="remove-vnet-peering"></a>Usuwanie wirtualnej sieci równorzędnej
1. W przeglądarce przejdź do strony http://portal.azure.com i w razie potrzeby zaloguj się przy użyciu konta platformy Azure.
2. Przejdź do bloku sieci wirtualnej, kliknij pozycję Peerings (Sesje komunikacji równorzędnej), kliknij połączenie, które chcesz usunąć, i kliknij przycisk Usuń.
   
   ![Usuwanie 1](./media/virtual-networks-create-vnetpeering-arm-portal/figure15.png)
3. Po usunięciu połączenia w wirtualnej sieci równorzędnej stan połączenia komunikacji równorzędnej zmieni się na Rozłączono.
   
    ![Usuwanie 2](./media/virtual-networks-create-vnetpeering-arm-portal/figure16.png)
4. W tym stanie nie można ponownie utworzyć połączenia, dopóki stan połączenia nie zmieni się na Zainicjowano. Zalecamy usunięcie obu połączeń przed ponownym utworzeniem wirtualnej sieci równorzędnej.




<!--HONumber=Nov16_HO2-->


