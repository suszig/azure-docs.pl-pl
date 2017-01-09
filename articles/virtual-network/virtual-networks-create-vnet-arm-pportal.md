---
title: "Tworzenie sieci wirtualnej przy użyciu witryny Azure Portal | Microsoft Docs"
description: "Dowiedz się, jak utworzyć sieć wirtualną przy użyciu witryny Azure Portal | Resource Manager."
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 4ad679a4-a959-4e48-a317-d9f5655a442b
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/8/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: ba7a67b8ae57da165f45bd3552a3dfac5f4ef64b
ms.openlocfilehash: c7257faad9a41174fb1d65e04c99cd96a8af3ea9


---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Tworzenie sieci wirtualnej przy użyciu witryny Azure Portal

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Platforma Azure ma dwa modele wdrażania: usługa Azure Resource Manager i wersja klasyczna. Firma Microsoft zaleca tworzenie zasobów za pomocą modelu wdrożenia usługi Resource Manager. Aby dowiedzieć się więcej o różnicach między dwoma modelami, zapoznaj się z artykułem [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) (Informacje na temat modeli wdrażania platformy Azure).
 
W tym artykule opisano sposób tworzenia sieci wirtualnej w modelu wdrożenia usługi Resource Manager w witrynie Azure Portal. Sieć wirtualną można również utworzyć w usłudze Resource Manager przy użyciu innych narzędzi albo za pośrednictwem klasycznego modelu wdrożenia, wybierając inną opcję z poniższej listy:

> [!div class="op_single_selector"]
- [Portal](virtual-networks-create-vnet-arm-pportal.md)
- [Program PowerShell](virtual-networks-create-vnet-arm-ps.md)
- [Interfejs wiersza polecenia](virtual-networks-create-vnet-arm-cli.md)
- [Szablon](virtual-networks-create-vnet-arm-template-click.md)
- [Portal (klasyczny)](virtual-networks-create-vnet-classic-pportal.md)
- [PowerShell (klasyczny)](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [Interfejs wiersza polecenia (klasyczny)](virtual-networks-create-vnet-classic-cli.md)


[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a>Tworzenie sieci wirtualnej

Aby utworzyć sieć wirtualną przy użyciu witryny Azure Portal, wykonaj następujące kroki:

1. W przeglądarce przejdź do strony http://portal.azure.com i w razie potrzeby zaloguj się przy użyciu konta platformy Azure.
2. Kliknij kolejno pozycje **Nowy** > **Sieć** > **Sieć wirtualna**, jak pokazano na poniższym rysunku:

    ![Nowa sieć wirtualna](./media/virtual-network-create-vnet-arm-pportal/1.png)

3. W wyświetlonym bloku **Sieć wirtualna** sprawdź, czy opcja *Resource Manager* została zaznaczona, a następnie kliknij pozycję **Utwórz**, jak pokazano na poniższym rysunku:

    ![Virtual Network](./media/virtual-network-create-vnet-arm-pportal/2.png)
    
4. W wyświetlonym bloku **Tworzenie sieci wirtualnej** wprowadź wartość *TestVNet* w polu **Nazwa**, wartość *192.168.0.0/16* w polu **Przestrzeń adresowa**, wartość *FrontEnd* w polu **Nazwa podsieci**, wartość *192.168.1.0/24* w polu **Zakres adresów podsieci** i wartość *TestRG* w polu **Grupa zasobów**. Wybierz swoją wartość pól **Subskrypcja** i **Lokalizacja**, a następnie kliknij przycisk **Utwórz**, jak pokazano na poniższym rysunku:

    ![Tworzenie sieci wirtualnej](./media/virtual-network-create-vnet-arm-pportal/3.png)

    Możesz również wybrać istniejącą grupę zasobów. Aby dowiedzieć się więcej na temat grup zasobów, zapoznaj się z artykułem [Omówienie usługi Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups). Możesz również wybrać inną lokalizację. Aby dowiedzieć się więcej o lokalizacjach i regionach świadczenia usługi Azure, zapoznaj się z artykułem poświęconym [regionom świadczenia usługi Azure](https://azure.microsoft.com/regions).

5. Portal pozwala tylko na utworzenie jednej podsieci podczas tworzenia sieci wirtualnej. W tym scenariuszu po utworzeniu sieci wirtualnej należy utworzyć drugą podsieć. Aby utworzyć drugą podsieć, kliknij pozycję **Wszystkie zasoby**, a następnie kliknij pozycję **TestVNet** w bloku **Wszystkie zasoby**, jak pokazano na poniższym rysunku:

    ![Utworzona sieć wirtualna](./media/virtual-network-create-vnet-arm-pportal/4.png)

6. W wyświetlonym bloku **TestVNet** kliknij kolejno pozycje **Podsieć** i **+ Podsieć**, wprowadź wartość *BackEnd* w polu **Nazwa** i wartość *192.168.2.0/24* w polu **Zakres adresów** w bloku **Dodawanie podsieci**, a następnie kliknij przycisk **OK**, jak pokazano na poniższym rysunku:

    ![Dodawanie podsieci](./media/virtual-network-create-vnet-arm-pportal/5.png)

7. Zostaną wyświetlone dwie podsieci, jak pokazano na poniższym rysunku:
    
    ![Lista podsieci w sieci wirtualnej](./media/virtual-network-create-vnet-arm-pportal/6.png)

W tym artykule wyjaśniono, jak utworzyć sieć wirtualną z dwiema podsieciami na potrzeby testowania. Przed utworzeniem sieci wirtualnej do użycia w środowisku produkcyjnym zalecamy zapoznanie się z artykułami [Omówienie usługi Virtual Network](virtual-networks-overview.md) i [Virtual network plan and design](virtual-network-vnet-plan-design-arm.md) (Projekt i plan sieci wirtualnej), aby w pełni zrozumieć działanie sieci wirtualnych i wszystkich ustawień. 

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak połączyć:

- Maszynę wirtualną z siecią wirtualną. Odpowiednie informacje znajdziesz w artykułach [Tworzenie maszyny wirtualnej z systemem Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md) lub [Tworzenie maszyny wirtualnej z systemem Linux](../virtual-machines/virtual-machines-linux-quick-create-portal.md). Zamiast tworzyć sieć wirtualną i podsieć, wykonując kroki opisane w artykułach, można wybrać istniejącą sieć wirtualną i podsieć, z którymi zostanie połączona maszyna wirtualna.
- Sieć wirtualną z innymi sieciami wirtualnymi. Odpowiednie informacje możesz znaleźć w artykule [Łączenie sieci wirtualnych](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).
- Sieć wirtualną z siecią lokalną za pomocą prywatnej sieci wirtualnej (VPN) typu lokacja-lokacja lub obwodu usługi ExpressRoute. Odpowiednie informacje znajdziesz w artykułach [Connect a VNet to an on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) (Łączenie sieci wirtualnej z siecią lokalną za pomocą sieci VPN typu lokacja-lokacja) i [Link a VNet to an ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md) (Łączenie sieci wirtualnej z obwodem usługi ExpressRoute).


<!--HONumber=Jan17_HO1-->


