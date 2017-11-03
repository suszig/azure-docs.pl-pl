---
title: "Klonowanie aplikacji sieci Web przy użyciu programu PowerShell"
description: "Dowiedz się, jak klonowanie aplikacji sieci Web do nowej aplikacji sieci Web przy użyciu programu PowerShell."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: f9a5cfa1-fbb0-41e6-95d1-75d457347a35
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2016
ms.author: aelnably
ms.openlocfilehash: dc252903571857b5fc89d1d9a2c63cd6b44e9021
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>Usługa aplikacji Azure aplikacji klonowania przy użyciu programu PowerShell
Wraz z wydaniem programu Microsoft Azure PowerShell w wersji 1.1.0 AzureRMWebApp nowy, która pozwoli uzyskać użytkownika klonowanie istniejącą aplikację sieci Web do aplikacji nowo utworzonej w innym regionie lub w tym samym regionie dodano nową opcję. Umożliwi to klienci mogą wdrożyć wiele aplikacji w różnych regionach, szybkie i łatwe.

Klonowanie aplikacji jest obecnie obsługiwany tylko w przypadku planów usługi aplikacji warstwy premium. Nowa funkcja używa tego samego ograniczenia jako funkcja kopii zapasowej aplikacji sieci Web, zobacz [kopii zapasowej aplikacji sieci web w usłudze Azure App Service](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>Klonowanie istniejącej aplikacji
Scenariusz: Istniejącej aplikacji sieci web w regionie południowo-środkowe stany, użytkownik chce klonowania zawartości do nowej aplikacji sieci web w regionie północno-środkowe Stany. Można to zrobić za pomocą polecenia cmdlet programu PowerShell za pośrednictwem usługi Azure Resource Manager do tworzenia nowej aplikacji sieci web z opcją - SourceWebApp.

Znajomość Nazwa grupy zasobów, która zawiera źródłowej aplikacji sieci web, możemy użyć następującego polecenia programu PowerShell można uzyskać informacji o źródłowej aplikacji sieci web (w tym przypadku o nazwie aplikacji sieci Web źródła):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Aby utworzyć nowy Plan usługi App Service, możemy użyć polecenia New-AzureRmAppServicePlan jak w poniższym przykładzie

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

Za pomocą polecenia New-AzureRmWebApp, możemy utworzyć nowej aplikacji sieci web w regionie północno-środkowe Stany i powiązanie jej istniejące warstwy premium planu usługi App Service, Ponadto możemy użyć tej samej grupy zasobów jako źródłowej aplikacji sieci web lub zdefiniuj nową grupę zasobów, pokazano poniżej:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

Klonowanie istniejącej aplikacji sieci web, w tym wszystkie skojarzone wdrażania gniazd, użytkownik będzie musiał użyć parametru IncludeSourceWebAppSlots następującego polecenia programu PowerShell pokazuje, użyj tego parametru przy użyciu polecenia New-AzureRmWebApp:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots

Można sklonować istniejącą aplikację sieci web w tym samym regionie, użytkownik będzie musiał utworzyć nową grupę zasobów i nowej usługi aplikacji planu w tym samym regionie, a następnie za pomocą następującego polecenia programu PowerShell na potrzeby klonowania aplikacji sieci web

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klonowanie istniejącej aplikacji do środowiska usługi aplikacji
Scenariusz: Istniejącej aplikacji sieci web w regionie południowo-środkowe stany, użytkownik chce klonowania zawartości do nowej aplikacji sieci web do istniejącej aplikacji usługi środowiska (ASE).

Znajomość Nazwa grupy zasobów, która zawiera źródłowej aplikacji sieci web, możemy użyć następującego polecenia programu PowerShell można uzyskać informacji o źródłowej aplikacji sieci web (w tym przypadku o nazwie aplikacji sieci Web źródła):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Znajomość ASE nazwy i nazwy grupy zasobów, której należy ASE, użytkownik może użyć polecenia New-AzureRmWebApp, aby utworzyć nową aplikację sieci web w istniejącej ASE, pokazano poniżej:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

Parametr lokalizacja jest wymagana powodu starszej wersji, ale w przypadku tworzenia aplikacji w elemencie ASE zostaną zignorowane. 

## <a name="cloning-an-existing-app-slot"></a>Klonowanie istniejącego miejsca aplikacji
Scenariusz: Użytkownik chce klonowania istniejącego miejsca aplikacji sieci Web albo w nowej aplikacji sieci Web lub nowe miejsce aplikacji sieci Web. W nowej aplikacji sieci Web może być w tym samym regionie co oryginalna miejsca aplikacji sieci Web lub w innym regionie.

Znajomość Nazwa grupy zasobów, która zawiera źródłowej aplikacji sieci web, możemy użyć następującego polecenia programu PowerShell można uzyskać informacji o miejsca aplikacji sieci web źródła (w tym przypadku o nazwie webappslot źródła) związana z aplikacji sieci Web aplikacji sieci Web źródła:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

Poniżej przedstawiono tworzenie własnego klonu źródłowej aplikacji sieci web nowej aplikacji sieci web:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Konfigurowanie usługi Traffic Manager podczas klonowania aplikacji
Tworzenie aplikacji sieci web w przypadku i konfigurowanie usługi Azure Traffic Manager można kierować ruchem do tych aplikacji sieci web, jest n scenariusza ważne, aby upewnić się, że aplikacje klientów są wysokiej dostępności, w przypadku klonowania istniejącej aplikacji sieci web należy opcję, aby połączyć obie aplikacje sieci web z nowego profilu Menedżera ruchu lub istniejącego - należy pamiętać, że tylko usługi Azure Resource Manager jest obsługiwana wersja usługi Traffic Manager.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Tworzenie nowego profilu Menedżera ruchu podczas klonowania aplikacji
Scenariusz: Użytkownik chce sklonować aplikacji sieci web do innego regionu, podczas konfigurowania profilu Menedżera ruchu usługi Azure Resource Manager, który obejmuje zarówno aplikacji sieci web. Poniżej przedstawiono tworzenie własnego klonu źródłowej aplikacji sieci web nowej aplikacji sieci web podczas konfigurowania profilu Menedżera ruchu:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Dodawanie nowych sklonować aplikacji sieci Web do istniejącego profilu Menedżera ruchu
Scenariusz: Użytkownik ma już profil Menedżera ruchu usługi Azure Resource Manager, które chciałby, aby dodać obie aplikacje sieci web jako punkty końcowe. Aby to zrobić, najpierw należy utworzyć istniejący identyfikator profilu Menedżera ruchu, są wymagane dla identyfikatora subskrypcji, nazwa grupy zasobów i istniejącej nazwy profilu Menedżera ruchu.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

Po identyfikatorze Menedżera ruchu, poniżej przedstawiono, tworzenie własnego klonu źródłowej aplikacji sieci web nowej aplikacji sieci web podczas dodawania ich do istniejącego profilu Menedżera ruchu:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Bieżące ograniczenia
Ta funkcja jest obecnie w przeglądzie, pracujemy, aby dodać nowe funkcje w czasie, poniżej są znane ograniczenia dotyczące bieżącej wersji w klonowania aplikacji:

* Ustawienia skalowania automatycznego nie są klonowane.
* Ustawienia harmonogramu tworzenia kopii zapasowej nie są klonowane.
* Ustawienia sieci Wirtualnej nie są klonowane.
* Wgląd w aplikację nie są automatycznie skonfigurowane na docelowej aplikacji sieci web
* Łatwe ustawienia uwierzytelniania nie są klonowane.
* Program kudu rozszerzenia nie są klonowane.
* Porada reguły nie są klonowane.
* Baza danych zawartości nie są klonowane.

### <a name="references"></a>Dokumentacja
* [Klonowanie aplikacji sieci Web](app-service-web-app-cloning.md)
* [Tworzenie kopii zapasowej aplikacji sieci web w usłudze Azure App Service](web-sites-backup.md)
* [Usługa Azure Resource Manager obsługę Azure Traffic Manager w wersji zapoznawczej](../traffic-manager/traffic-manager-powershell-arm.md)
* [Wprowadzenie do usługi App Service Environment](environment/intro.md)
* [Używanie programu Azure PowerShell z usługą Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)

