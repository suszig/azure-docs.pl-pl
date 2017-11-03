---
title: "Zmodyfikuj prefiksy adresów IP bramy sieci lokalnej i adres IP bramy sieci VPN | Azure | INTERFEJS WIERSZA POLECENIA | Dokumentacja firmy Microsoft"
description: "W tym artykule przedstawiono zmiana prefiksów adresów IP dla bramy sieci lokalnej przy użyciu wiersza polecenia platformy Azure."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8c7db48f-d09a-44e7-836f-1fb6930389df
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: cherylmc
ms.openlocfilehash: 7db1ad970ebb93d46d5a861f9a9b27bf121531a3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="modify-local-network-gateway-settings-using-the-azure-cli"></a>Zmodyfikuj ustawienia bramy sieci lokalnej przy użyciu wiersza polecenia platformy Azure

Czasami zmienić ustawienia dla bramy sieci lokalnej prefiks adresu lub adres IP bramy. W tym artykule przedstawiono sposób zmodyfikować ustawienia bramy sieci lokalnej. Można również zmodyfikować te ustawienia przy użyciu innej metody, wybierając inną opcję z poniższej listy:

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](vpn-gateway-modify-local-network-gateway-portal.md)
> * [PowerShell](vpn-gateway-modify-local-network-gateway.md)
> * [Interfejs wiersza polecenia platformy Azure](vpn-gateway-modify-local-network-gateway-cli.md)
>
>

## <a name="before"></a>Przed rozpoczęciem

Zainstaluj najnowszą wersję polecenia interfejsu wiersza polecenia (2.0 lub nowszej). Aby uzyskać informacje o instalowaniu poleceń interfejsu wiersza polecenia, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure w wersji 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).

[!INCLUDE [CLI-login](../../includes/vpn-gateway-cli-login-include.md)]

## <a name="ipaddprefix"></a>Zmodyfikuj prefiksy adresów IP

[!INCLUDE [modify-prefix](../../includes/vpn-gateway-modify-ip-prefix-cli-include.md)]

## <a name="gwip"></a>Zmodyfikuj adres IP bramy

[!INCLUDE [modify-gateway-IP](../../includes/vpn-gateway-modify-lng-gateway-ip-cli-include.md)]

## <a name="next-steps"></a>Następne kroki

Można sprawdzić połączenie bramy. Zobacz [sprawdzić połączenie bramy](vpn-gateway-verify-connection-resource-manager.md).

