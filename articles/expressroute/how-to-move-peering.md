---
title: "Przenieś publicznej komunikacji równorzędnej Azure ExpressRoute do komunikacji równorzędnej firmy Microsoft | Dokumentacja firmy Microsoft"
description: "W tym artykule przedstawiono kroki, aby przenieść sieci publicznej komunikacji równorzędnej do firmy Microsoft komunikacji równorzędnej ExpressRoute."
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/07/2017
ms.author: cherylmc
ms.openlocfilehash: 311e1de3200cd684bbec1329ebd5217b4fb3a2e2
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2017
---
# <a name="move-a-public-peering-to-microsoft-peering"></a>Przenieś publicznej komunikacji równorzędnej się do komunikacji równorzędnej firmy Microsoft

ExpressRoute obsługuje teraz usług Azure PaaS, takich jak magazynu Azure i usługi Azure SQL Database za pomocą komunikacji równorzędnej z filtrami tras firmy Microsoft. Teraz należy tylko jednej domeny routingu dostęp do usług SaaS i PaaS firmy Microsoft. Można korzystać z filtrów tras selektywne anonsowanie prefiksy usługi PaaS w regionach platformy Azure, który chcesz wykorzystać.

W tym artykule opisano, jak przenieść publicznej konfiguracji komunikacji równorzędnej firmy Microsoft, równorzędna bez przestojów. Aby uzyskać więcej informacji o routingu domeny i komunikacji równorzędnych, zobacz [ExpressRoute obwody i domeny routingu](expressroute-circuit-peerings.md).

> [!IMPORTANT]
> Aby można było używać komunikacji równorzędnej firmy Microsoft, musi mieć dodatek usługi ExpressRoute w warstwie premium. Aby uzyskać więcej informacji na temat dodatek premium, zobacz [ExpressRoute — często zadawane pytania](expressroute-faqs.md#expressroute-premium).

## <a name="before-you-begin"></a>Przed rozpoczęciem

* Aby nawiązać komunikacji równorzędnej firmy Microsoft, należy skonfigurować i zarządzać nimi translatora adresów sieciowych. Dostawca połączenia może oraz zarządzać nimi translatora adresów Sieciowych jako usługa zarządzana. Jeśli planujesz dostęp do usług Azure SaaS w komunikacji równorzędnej firmy Microsoft i Azure PaaS, należy poprawnie rozmiaru puli adresów IP translatora adresów Sieciowych. Aby uzyskać więcej informacji o NAT ExpressRoute, zobacz [NAT wymagania dotyczące komunikacji równorzędnej firmy Microsoft](expressroute-nat.md#nat-requirements-for-microsoft-peering).

* Jeśli aktualnie sieci listę kontroli dostępu (ACL) dla punktów końcowych usługi Azure PaaS podczas korzystania z publicznej komunikacji równorzędnej platformy Azure należy upewnić się, że adres IP translatora adresów Sieciowych skonfigurowano puli komunikacji równorzędnej firmy Microsoft znajduje się na liście kontroli dostępu skonfigurowane dla usługi punkt końcowy.

Aby przenieść do firmy Microsoft, równorzędna bez przestojów, musi być kroki opisane w tym artykule w kolejności, które mają być przedstawiane.

## <a name="1-create-microsoft-peering"></a>1. Utwórz komunikacji równorzędnej firmy Microsoft

Jeśli komunikacji równorzędnej firmy Microsoft nie został utworzony, należy użyć dowolnego z następujących artykułów utworzyć komunikacji równorzędnej firmy Microsoft. Jeśli oferty dostawcy łączności zarządzane warstwy 3 usługi, poproś dostawca łączności do włączenia komunikacji równorzędnej dla obwodu firmy Microsoft.

  * [Utwórz komunikacji równorzędnej firmy Microsoft przy użyciu portalu Azure](expressroute-howto-routing-portal-resource-manager.md#msft)
  * [Utwórz komunikacji równorzędnej firmy Microsoft przy użyciu programu Azure Powershell](expressroute-howto-routing-arm.md#msft)
  * [Utwórz komunikacji równorzędnej firmy Microsoft przy użyciu wiersza polecenia platformy Azure](howto-routing-cli.md#msft)

## <a name="2-validate-microsoft-peering-is-enabled"></a>2. Sprawdź poprawność Microsoft równorzędna jest włączona

Sprawdź, czy włączono komunikacji równorzędnej firmy Microsoft i anonsowany prefiksów publicznych znajdują się w stanie skonfigurowany.

  * [Witryna Azure Portal](expressroute-howto-routing-portal-resource-manager.md#getmsft)
  * [Azure PowerShell](expressroute-howto-routing-arm.md#getmsft)
  * [Interfejs wiersza polecenia platformy Azure](howto-routing-cli.md#getmsft)

## <a name="3-configure-and-attach-a-route-filter-to-the-circuit"></a>3. Konfigurowanie i dołączanie filtr trasy do obwodu

Domyślnie nowe Microsoft komunikacji równorzędnych nie anonsuje wszystkie prefiksy dopóki filtr tras jest dołączony do obwodu. Podczas tworzenia reguły filtru trasy, można określić listy Wspólnot usługi, które regiony platformy Azure, które chcesz korzystać z usług Azure PaaS, jak pokazano na poniższym zrzucie ekranu:

![Scal publicznej komunikacji równorzędnej](.\media\how-to-move-peering\public.png)

Użyj dowolnego z poniższych artykułów, aby skonfigurować filtry tras.

  * [Konfigurowanie filtrów trasy dla komunikacji równorzędnej firmy Microsoft przy użyciu portalu Azure](how-to-routefilter-portal.md)
  * [Konfigurowanie filtrów trasy dla komunikacji równorzędnej firmy Microsoft przy użyciu programu Azure PowerShell](how-to-routefilter-powershell.md)
  * [Konfigurowanie filtrów trasy dla komunikacji równorzędnej firmy Microsoft przy użyciu wiersza polecenia platformy Azure](how-to-routefilter-cli.md)

## <a name="4-delete-the-public-peering"></a>4. Usuń publicznej komunikacji równorzędnej

Po zweryfikowaniu, że skonfigurowano komunikacji równorzędnej firmy Microsoft i prefiksy, który chcesz wykorzystać są poprawnie anonsowane w komunikacji równorzędnej firmy Microsoft, możesz następnie usunąć publicznej komunikacji równorzędnej. Aby usunąć publicznej komunikacji równorzędnej, należy użyć dowolnego z następujących artykułów:

  * [Usuń publicznej komunikacji równorzędnej platformy Azure przy użyciu portalu Azure](expressroute-howto-routing-portal-resource-manager.md#deletepublic)
  * [Usuń publicznej komunikacji równorzędnej platformy Azure przy użyciu programu Azure PowerShell](expressroute-howto-routing-arm.md#deletepublic)
  * [Usuń publicznej komunikacji równorzędnej platformy Azure przy użyciu interfejsu wiersza polecenia](howto-routing-cli.md#deletepublic)

## <a name="next-steps"></a>Następne kroki

Więcej informacji na temat usługi ExpressRoute znajduje się w artykule [ExpressRoute FAQ](expressroute-faqs.md) (Usługa ExpressRoute — często zadawane pytania).