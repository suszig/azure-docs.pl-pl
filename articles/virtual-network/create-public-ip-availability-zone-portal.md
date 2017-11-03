---
title: "Tworzenie zoned publiczny adres IP za pomocą portalu Azure | Dokumentacja firmy Microsoft"
description: "Utwórz publiczny adres IP w strefie dostępności przy użyciu portalu Azure."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 2fcbed2f83d66a0b4336cd1c464bb02eff3ef229
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-public-ip-address-in-an-availability-zone-with-the-azure-portal"></a>Tworzenie publicznego adresu IP w strefie dostępności przy użyciu portalu Azure

Można wdrożyć publicznego adresu IP w strefie dostępności Azure (wersja zapoznawcza). Strefa dostępności jest fizycznie oddzielnych stref w regionie platformy Azure. Instrukcje:

> * Tworzenie publicznego adresu IP w strefie dostępności
> * Zidentyfikuj powiązane zasoby utworzone w strefie dostępności

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

> [!NOTE]
> Dostępność strefy są w wersji zapoznawczej i są gotowe do programowania i testowania scenariuszy. Obsługa jest dostępna dla wybierz zasobów platformy Azure i regiony i rodziny rozmiar maszyny Wirtualnej. Więcej informacji na temat rozpocząć, które zasobów platformy Azure, regiony i rodziny rozmiar maszyny Wirtualnej można spróbować stref dostępności przy, zobacz [Przegląd stref dostępności](https://docs.microsoft.com/azure/availability-zones/az-overview). Aby uzyskać pomoc techniczną, możesz skorzystać z witryny [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) lub [otworzyć bilet pomocy technicznej platformy Azure](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do portalu Azure pod adresem https://portal.azure.com. 

## <a name="create-a-zonal-public-ip-address"></a>Utwórz zonal publicznego adresu IP

1. Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.
2. Wybierz **sieci**, a następnie wybierz **publicznego adresu IP**.
3. Wprowadź lub wybierz wartości poniższych ustawień, wybierz subskrypcję, zaakceptuj ustawienia domyślne dla pozostałych ustawień, a następnie kliknij przycisk **Utwórz**:

    |Ustawienie|Wartość|
    |---|---|
    |SKU| **Podstawowe**: przypisane przy użyciu metody statyczne lub dynamiczne alokacji. Mogą być przypisane do zasobów platformy Azure, które mogą być przypisane do publicznego adresu IP, takich jak interfejsów sieciowych, bram sieci VPN bramy aplikacji i usług równoważenia obciążenia internetowy. Publiczny adres IP można przypisać do określonej strefy w **strefy dostępności** ustawienie. Nie są strefowo nadmiarowy. Aby dowiedzieć się więcej o strefach dostępności, zobacz [Availability zones overview (Omówienie stref dostępności)](https://docs.microsoft.com/azure/availability-zones/az-overview). **Standardowe**: przypisane przy użyciu metody statycznej alokacji. Mogą być przypisane do interfejsów sieciowych lub standardowy internetowy usług równoważenia obciążenia. Po przypisaniu publicznego adresu IP do standardowego modułu równoważenia obciążenia internetowy, musisz wybrać Standard. Aby uzyskać więcej informacji na temat jednostek SKU usługi Azure Load Balancer, zobacz [Azure load balancer standard SKU (Usługa Azure Load Balancer — standardowa jednostka SKU)](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview). Są strefowo nadmiarowy, domyślnie. Mogą być tworzone strefowo i być gwarantowane w określonej strefie dostępności. Standardowa jednostka SKU jest dostępna w wersji zapoznawczej. Przed utworzeniem standardowy SKU publiczny adres IP, najpierw należy zarejestrować skorzystania z wersji zapoznawczej. Aby zarejestrować się do korzystania z wersji zapoznawczej, zobacz część [Register for the standard SKU preview (Rejestrowanie się do korzystania ze standardowej jednostki SKU w wersji zapoznawczej)](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#preview-sign-up). Standardowy SKU można tworzyć tylko w obsługiwanych lokalizacji.  Aby dowiedzieć się więcej na temat obsługi dodatkowych regionów, zapoznaj się z listą obsługiwanych lokalizacji (regionów) w sekcji [Region availability (Dostępność w regionach)](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview#region-availability) i monitoruj stronę [aktualizacji usługi Azure Virtual Network](https://azure.microsoft.com/updates/?product=virtual-network).|   
    |Nazwa|Nazwa musi być unikatowa w ramach grupy zasobów, którą wybierzesz.|
    |Grupa zasobów|Kliknij przycisk Utwórz nowy, a następnie wprowadź myResourceGroup|
    |Lokalizacja|Europa Zachodnia|
    |Dostępność strefy|W przypadku wybrania **standardowe** jednostka SKU, możesz wybrać *obszar strefowo nadmiarowy* Jeśli adres IP pozwala uzyskać odporność w różnych strefach. W przypadku wybrania **podstawowe** jednostka SKU, adres IP nie jest odporność różnych strefach. Niezależnie od wybranej jednostki SKU można przypisać adres do określonej strefy, wybranie opcji. |

    Ustawienia będą wyświetlane w portalu, jak pokazano na poniższej ilustracji:

    ![Utwórz zonal publicznego adresu IP](./media/create-public-ip-availability-zone-portal/public-ip-address.png) 

> [!NOTE]
> Po przypisaniu standardowego publicznego adresu IP jednostki SKU do interfejsu sieciowego maszyny wirtualnej musisz jawnie zezwolić na ruch do miejsca przeznaczenia przy użyciu [sieciowej grupy zabezpieczeń](security-overview.md#network-security-groups). Próba komunikacji z zasobem będzie kończyć się niepowodzeniem do momentu utworzenia i skojarzenia sieciowej grupy zabezpieczeń, a następnie jawnego zezwolenia na żądany ruch.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [stref dostępności](https://docs.microsoft.com/azure/availability-zones/az-overview)
- Dowiedz się więcej o [publicznych adresów IP](virtual-network-public-ip-address.md?toc=%2fazure%2fvirtual-network%2ftoc.json)