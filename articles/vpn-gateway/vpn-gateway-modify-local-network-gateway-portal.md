---
title: "Zmodyfikuj prefiksy adresów IP bramy sieci lokalnej i adres IP bramy sieci VPN | Azure | Portal | Dokumentacja firmy Microsoft"
description: "W tym artykule przedstawiono zmiana prefiksów adresów IP dla bramy sieci lokalnej przy użyciu portalu Azure."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: bdd6f90fe97408bd45a72adf58bfdc190207de46
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="modify-local-network-gateway-settings-using-the-azure-portal"></a>Zmodyfikuj ustawienia bramy sieci lokalnej za pomocą portalu Azure

Czasami zmienić ustawienia dla bramy sieci lokalnej prefiks adresu lub GatewayIPAddress. W tym artykule przedstawiono sposób zmodyfikować ustawienia bramy sieci lokalnej. Można również zmodyfikować te ustawienia przy użyciu innej metody, wybierając inną opcję z poniższej listy:

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Interfejs wiersza polecenia platformy Azure](vpn-gateway-modify-local-network-gateway-cli.md)
>
>


## <a name="ipaddprefix"></a>Zmodyfikuj prefiksy adresów IP

Po zmodyfikowaniu prefiksów adresów IP, kroki, które należy wykonać są zależne od tego, czy brama sieci lokalnej ma połączenie.

[!INCLUDE [modify prefix](../../includes/vpn-gateway-modify-ip-prefix-portal-include.md)]

## <a name="gwip"></a>Zmodyfikuj adres IP bramy

W przypadku zmiany publicznego adresu IP urządzenia sieci VPN, z którym chcesz nawiązać połączenie, musisz zmodyfikować bramę sieci lokalnej w celu odzwierciedlenia tej zmiany. Po zmianie publicznego adresu IP, kroki, które należy wykonać są zależne od tego, czy brama sieci lokalnej ma połączenie.

[!INCLUDE [modify gateway IP](../../includes/vpn-gateway-modify-lng-gateway-ip-portal-include.md)]

## <a name="next-steps"></a>Następne kroki

Można sprawdzić połączenie bramy. Zobacz [sprawdzić połączenie bramy](vpn-gateway-verify-connection-resource-manager.md).