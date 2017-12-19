---
title: "Połączyć sieć wirtualną z obwodem usługi ExpressRoute: portalu Azure | Dokumentacja firmy Microsoft"
description: "Ten dokument zawiera omówienie sposobu łączenia sieci wirtualnych (sieci wirtualne) z obwody usługi ExpressRoute."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f5cb5441-2fba-46d9-99a5-d1d586e7bda4
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: cherylmc
ms.openlocfilehash: 34caed9323ae3067d1dc17ab9c62ebf7a9be855b
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit"></a>Połączenie wirtualnej sieci do obwodu usługi ExpressRoute
> [!div class="op_single_selector"]
> * [Witryna Azure Portal](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-linkvnet-arm.md)
> * [Interfejs wiersza polecenia platformy Azure](howto-linkvnet-cli.md)
> * [Video - portalu Azure](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> * [PowerShell (klasyczny)](expressroute-howto-linkvnet-classic.md)
> 

Ten artykuł pomaga połączyć sieci wirtualnych (sieci wirtualne) do usługi Azure ExpressRoute obwodów przy użyciu modelu wdrażania usługi Resource Manager i portalu Azure. Sieci wirtualne może być w tej samej subskrypcji, lub mogą być częścią innej subskrypcji.

## <a name="before-you-begin"></a>Przed rozpoczęciem
* Przegląd [wymagania wstępne](expressroute-prerequisites.md), [wymagania dotyczące routingu](expressroute-routing.md), i [przepływy pracy](expressroute-workflows.md) przed rozpoczęciem konfigurowania.
* Musisz mieć aktywny obwód usługi ExpressRoute.
  
  * Postępuj zgodnie z instrukcjami, aby [utworzyć obwodu usługi ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) i mieć obwodu włączane przez dostawcą połączenia.
  * Upewnij się, że masz prywatnej komunikacji równorzędnej platformy Azure skonfigurowane dla obwodu. Zobacz [Konfigurowanie routingu](expressroute-howto-routing-portal-resource-manager.md) artykułu instrukcje routingu.
  * Upewnij się, że Azure prywatnej komunikacji równorzędnej jest skonfigurowana i komunikację równorzędną BGP między siecią a Microsoft jest uruchomiony, dzięki czemu można włączyć łączność end-to-end.
  * Upewnij się, że masz sieci wirtualnej i Brama sieci wirtualnej utworzone i w pełni zaaprowizowanym. Postępuj zgodnie z instrukcjami, aby [Tworzenie bramy sieci wirtualnej dla usługi ExpressRoute](expressroute-howto-add-gateway-resource-manager.md). Brama sieci wirtualnej dla usługi ExpressRoute używa elementu GatewayType "ExpressRoute", nie sieci VPN.

* Standardowa obwodu ExpressRoute można połączyć maksymalnie 10 sieciami wirtualnymi. Wszystkie wirtualne sieci musi być w tym samym regionie geograficznymi, gdy przy użyciu standardowych obwodu usługi expressroute. 
* Można połączyć sieć wirtualną poza region geograficznymi obwodu ExpressRoute lub większą liczbę sieci wirtualnych połączyć się z obwodu ExpressRoute włączenie dodatek usługi ExpressRoute w warstwie premium. Sprawdź [— często zadawane pytania](expressroute-faqs.md) uzyskać więcej informacji dotyczących dodatek w warstwie premium.
* Możesz [wyświetlanie wideo](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit) przed rozpoczęciem lepiej zrozumieć kroki.

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Połączyć się z obwodem sieci wirtualnej w tej samej subskrypcji

### <a name="to-create-a-connection"></a>Aby utworzyć połączenie

> [!NOTE]
> Informacje o konfiguracji protokołu BGP, nie pojawiają się jeśli dostawca warstwy 3 skonfigurowany z komunikacji równorzędnych. Jeśli obwodu jest w stanie udostępnione, należy utworzyć połączenia.
>

1. Upewnij się, że obwód usługi expressroute i Azure prywatnej komunikacji równorzędnej skonfigurowano pomyślnie. Postępuj zgodnie z instrukcjami [utworzyć obwodu usługi ExpressRoute](expressroute-howto-circuit-arm.md) i [Konfigurowanie routingu](expressroute-howto-routing-arm.md). Obwodu ExpressRoute powinien wyglądać podobnie jak na poniższej ilustracji:

    ![Zrzut ekranu obwodu usługi ExpressRoute](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)
   
2. Można teraz uruchomić inicjowania obsługi połączenia do połączenia bramy sieci wirtualnej do obwodu usługi ExpressRoute. Kliknij przycisk **połączenia** > **Dodaj** otworzyć **Dodaj połączenie** bloku, a następnie skonfigurować wartości.

    ![Dodaj połączenie zrzut ekranu](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)  

3. Po pomyślnym skonfigurowaniu połączenia obiekt połączenia zostaną wyświetlone informacje dotyczące połączenia.

     ![Zrzut ekranu obiektu połączenia](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)

### <a name="to-delete-a-connection"></a>Aby usunąć połączenie
Połączenie można usunąć, wybierając **usunąć** ikonę w bloku dla połączenia.

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Połączenie sieci wirtualnej w innej subskrypcji z obwodem
Obwodu usługi ExpressRoute mogą udostępniać między wieloma subskrypcjami. Na poniższym rysunku przedstawiono prostą przedstawienie sposobu udostępniania prac dla obwody usługi ExpressRoute między wieloma subskrypcjami.

![Łącznością między subskrypcjami](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- Każdy z mniejszym chmury w chmurze dużych jest używana do reprezentowania subskrypcje, które należą do różnych działów w organizacji.
- Każdego z działów w organizacji można używać własnej subskrypcji do wdrażania usług, ale mogą współużytkować jeden obwodu ExpressRoute do nawiązywania ponownego połączenia z siecią lokalną.
- Jednego działu (w tym przykładzie: IT) może być właścicielem obwodu usługi expressroute. Inne subskrypcje w organizacji można używać obwodu usługi expressroute i autoryzacje skojarzone z obwodem, łącznie z subskrypcji połączone z innymi dzierżawcami usługi Azure Active Directory i rejestracji Enterprise Agreement. 

    > [!NOTE]
    > Połączeniami i przepustowością opłat za obwód dedykowany zostaną zastosowane do właściciela obwodu usługi ExpressRoute. Wszystkie sieci wirtualne udostępnianie tego samego przepustowości.
    > 
    >

### <a name="administration---circuit-owners-and-circuit-users"></a>Administrowanie — obwodu właścicieli i użytkowników obwodu

Właściciela obwodu jest autoryzowanym użytkownikiem zasilania zasobu obwodu usługi ExpressRoute. Właściciel obwodu można utworzyć autoryzacje, które można wykorzystać przez "obwód użytkowników". Użytkownicy obwodu są właścicielami bram sieci wirtualnej, które nie są w tej samej subskrypcji co obwodu usługi expressroute. Użytkownicy obwodu zrealizować autoryzacje (jeden autoryzacji dla sieci wirtualnej).

Właściciel obwodu ma uprawnienia do modyfikowania i odwoływanie autoryzacje w dowolnym momencie. Odwoływanie powoduje autoryzacji wszystkich połączeń usuwany z subskrypcji, do których dostęp został odwołany.

### <a name="circuit-owner-operations"></a>Operacje właściciela obwodu

**Aby utworzyć autoryzacji połączenia**

Właściciel obwodu tworzy autoryzacji. Powoduje to utworzenie klucza autoryzacji, którego użytkownik obwodu można nawiązać ich bram sieci wirtualnej z obwodem usługi ExpressRoute. Autoryzacji dotyczy tylko jedno połączenie.

1. W bloku ExpressRoute kliknij **autoryzacje** , a następnie wpisz **nazwa** autoryzacji i kliknij **zapisać**.

    ![Autoryzacje](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)

2. Po zapisaniu konfiguracji Skopiuj **identyfikator zasobu** i **klucza autoryzacji**.

    ![Klucz autoryzacji](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

**Aby usunąć autoryzacji połączenia**

Połączenie można usunąć, wybierając **usunąć** ikonę w bloku dla połączenia.

### <a name="circuit-user-operations"></a>Operacje użytkownik obwodu

Użytkownik obwodu musi identyfikator zasobu i klucza autoryzacji właściciela obwodu. 

**Aby zrealizować autoryzacji połączenia**

1.  Kliknij przycisk **+ nowy** przycisku.

    ![Kliknij przycisk Nowy](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)

2.  Wyszukaj **"Połączenia"** w witrynie Marketplace, zaznacz go, a następnie kliknij przycisk **Utwórz**.

    ![Wyszukiwanie połączenia](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)

3.  Upewnij się, że **typ połączenia** ma ustawioną wartość "ExpressRoute".


4.  Wypełnij szczegóły, a następnie kliknij przycisk **OK** w bloku podstawowe informacje.

    ![Blok Podstawowe](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)

5.  W **ustawienia** bloku, wybierz **Brama sieci wirtualnej** i sprawdź **zrealizować autoryzacji** pole wyboru.

6.  Wprowadź **klucza autoryzacji** i **elementu równorzędnego obwodu URI** i nadaj nazwę połączenia. Kliknij przycisk **OK**.

    ![Blok Ustawienia](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)

7. Przejrzyj informacje w **Podsumowanie** bloku i kliknij przycisk **OK**.


**Aby zwolnić autoryzacji połączenia**

Można zwolnić autoryzacji, usuwając połączenia prowadzący obwodu ExpressRoute do sieci wirtualnej.

## <a name="next-steps"></a>Następne kroki
Więcej informacji na temat usługi ExpressRoute znajduje się w artykule [ExpressRoute FAQ](expressroute-faqs.md) (Usługa ExpressRoute — często zadawane pytania).
