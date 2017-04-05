---
title: "Nawiązywanie połączenia z maszyną wirtualną z systemem Windows Server | Microsoft Docs"
description: "Dowiedz się, jak nawiązać połączenie z maszyną wirtualną z systemem Windows oraz zalogować się do niej za pomocą witryny Azure Portal i modelu wdrażania przy użyciu usługi Azure Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ef62b02e-bf35-468d-b4c3-71b63fe7f409
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/01/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 88431377a36d5bc36220c630f0c8d4a46ab4a434
ms.lasthandoff: 03/31/2017


---
# <a name="how-to-connect-and-log-on-to-an-azure-virtual-machine-running-windows"></a>Sposób nawiązywania połączenia z maszyną wirtualną platformy Azure z systemem Windows oraz logowania się do niej
Korzystając z przycisku **Połącz** w witrynie Azure Portal, uruchomisz sesję pulpitu zdalnego z poziomu pulpitu systemu Windows. Najpierw nawiążesz połączenie z maszyną wirtualną, a następnie zalogujesz się.

W przypadku próby nawiązania połączenia z maszyną wirtualną z systemem Windows z poziomu komputera Mac konieczne jest zainstalowanie klienta RDP dla komputerów Mac, takiego jak [Pulpit zdalny Microsoft](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).

## <a name="connect-to-the-virtual-machine"></a>Nawiązywanie połączenia z maszyną wirtualną
1. Jeśli jeszcze tego nie zrobiono, zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
2. W menu Centrum kliknij pozycję **Virtual Machines**.
3. Wybierz maszynę wirtualną z listy.
4. W bloku maszyny wirtualnej kliknij pozycję **Połącz**.
   
    ![Zrzut ekranu witryny Azure Portal pokazujący sposób nawiązywania połączenia z maszyną wirtualną.](./media/connect-logon/connect.png)
   
   > [!TIP]
   > Jeśli przycisk **Połącz** w portalu jest nieaktywny i nie masz połączenia typu [ExpressRoute](../../expressroute/expressroute-introduction.md) lub Sieć VPN typu [lokacja do lokacji](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) z platformą Azure, musisz utworzyć i przypisać maszynie wirtualnej publiczny adres IP przed użyciem pulpitu zdalnego. Więcej informacji o [publicznych adresach IP na platformie Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).
   > 
   > 

## <a name="log-on-to-the-virtual-machine"></a>Logowanie do maszyny wirtualnej
[!INCLUDE [virtual-machines-log-on-win-server](../../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Następne kroki
Jeśli podczas próby połączenia wystąpiły problemy, zobacz [Troubleshoot Remote Desktop connections](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Rozwiązywanie problemów z połączeniami pulpitu zdalnego). W tym artykule przedstawiono sposób diagnozowania i rozwiązywania typowych problemów.


