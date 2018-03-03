---
title: "Zainstaluj program Visual Studio i połącz się stosu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się na krokach wymaganych do instalacji programu Visual Studio i połącz się stosu Azure"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: mabrigg
ms.openlocfilehash: 5eae2553af1e41ace878a2f8f2c1a1656c63a7c4
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>Zainstaluj program Visual Studio i połącz się stosu Azure

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Tworzenie i wdrażanie usługi Azure Resource Manager za pomocą programu Visual Studio [szablony](azure-stack-arm-templates.md) w stosie Azure. Korzystając z procedury opisanej w tym artykule można zainstalować program Visual Studio z [Azure stosu Development Kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), lub z systemem Windows klient zewnętrznych po nawiązaniu połączenia za pośrednictwem [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn). Te kroki nowej instalacji programu Visual Studio 2015 Community Edition. Przeczytaj więcej na temat [współistnienie](https://msdn.microsoft.com/library/ms246609.aspx) między inne wersje programu Visual Studio.

## <a name="install-visual-studio"></a>Instalacja programu Visual Studio
1. Pobierz i uruchom [Instalatora platformy sieci Web](https://www.microsoft.com/web/downloads/platform.aspx).             
2. Wyszukaj **Visual Studio Community 2015 z zestawem Microsoft Azure SDK - 2.9.6**, kliknij przycisk **Dodaj**, i **zainstalować**.

    ![Zrzut ekranu WebPI kroki instalacji](./media/azure-stack-install-visual-studio/image1.png) 

3. Odinstaluj **Microsoft Azure PowerShell** który jest instalowany jako część zestawu SDK platformy Azure.

    ![Zrzut ekranu przedstawiający Dodaj/Usuń programy interfejs dla programu Azure PowerShell](./media/azure-stack-install-visual-studio/image2.png) 

4. [Install PowerShell for Azure Stack](azure-stack-powershell-install.md) (Instalowanie programu PowerShell dla usługi Azure Stack)

5. Po zakończeniu instalacji uruchom ponownie system operacyjny.

## <a name="connect-to-azure-stack"></a>Nawiązywanie połączenia z usługą Azure Stack

1. Uruchom program Visual Studio.

2. Z **widoku** menu, wybierz opcję **Eksplorator chmury**.

3. W okienku, wybierz **Dodaj konto** i zaloguj się przy użyciu poświadczeń usługi Azure Active Directory.  
    ![Zrzut ekranu Eksplorator chmury raz zalogowany i podłączony do stosu Azure](./media/azure-stack-install-visual-studio/image6.png)

Po zalogowaniu, możesz [wdrażanie szablonów](azure-stack-deploy-template-visual-studio.md) lub przeglądanie dostępnych typów zasobów i grup zasobów, aby tworzyć własne szablony.  

## <a name="next-steps"></a>Następne kroki

 - [Tworzenie szablonów dla stosu Azure](azure-stack-develop-templates.md)
