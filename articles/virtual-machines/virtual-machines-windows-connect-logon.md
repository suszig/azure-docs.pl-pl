<properties
    pageTitle="Nawiązywanie połączenia z maszyną wirtualną z systemem Windows Server | Microsoft Azure"
    description="Dowiedz się, jak nawiązać połączenie z maszyną wirtualną z systemem Windows oraz zalogować się do niej za pomocą witryny Azure Portal i modelu wdrażania przy użyciu usługi Azure Resource Manager."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/28/2016"
    ms.author="cynthn"/>


# Sposób nawiązywania połączenia z maszyną wirtualną platformy Azure z systemem Windows oraz logowania się do niej 


Korzystając z przycisku **Połącz** w witrynie Azure Portal, uruchomisz sesję pulpitu zdalnego. Najpierw nawiążesz połączenie z maszyną wirtualną, a następnie zalogujesz się.

## Nawiązywanie połączenia z maszyną wirtualną

1. Jeśli jeszcze tego nie zrobiono, zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).

2.  W menu Centrum kliknij pozycję **Virtual Machines**.

3.  Wybierz maszynę wirtualną z listy.

4. W bloku maszyny wirtualnej kliknij pozycję **Połącz**.

    ![Zrzut ekranu witryny Azure Portal pokazujący sposób nawiązywania połączenia z maszyną wirtualną.](./media/virtual-machines-windows-connect-logon/connect.png)
    
 > [AZURE.TIP] Jeśli przycisk **Połącz** w portalu jest nieaktywny i nie masz połączenia typu [ExpressRoute](../expressroute/expressroute-introduction.md) lub Sieć VPN typu [lokacja do lokacji](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) z platformą Azure, musisz utworzyć i przypisać maszynie wirtualnej publiczny adres IP przed użyciem pulpitu zdalnego. Więcej informacji o [publicznych adresach IP na platformie Azure](../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## Logowanie do maszyny wirtualnej

[AZURE.INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]


## Następne kroki

Jeśli podczas próby połączenia wystąpiły problemy, zobacz [Troubleshoot Remote Desktop connections](virtual-machines-windows-troubleshoot-rdp-connection.md) (Rozwiązywanie problemów z połączeniami pulpitu zdalnego). W tym artykule przedstawiono sposób diagnozowania i rozwiązywania typowych problemów.



<!--HONumber=Sep16_HO3-->


