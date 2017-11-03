---
title: "Konfigurowanie warstwy PremiumV2 dla usługi Azure App Service | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak lepszej wydajności sieci web, mobilnych i aplikacji interfejsu API w usłudze Azure App Service przez skalowanie do nowego PremiumV2 warstwy cenowej."
keywords: app service, azure app service, scale, scalable, app service plan, app service cost
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: ff00902b-9858-4bee-ab95-d3406018c688
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: cephalin
ms.openlocfilehash: 92cc8d8b0f67dde95ea2e3fc2f0f083bd8ac8aab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-premiumv2-tier-for-azure-app-service"></a>Konfigurowanie warstwy PremiumV2 dla usługi Azure App Service

Nowy **PremiumV2** warstwa cenowa oferuje [Dv2 serii maszyn wirtualnych](../virtual-machines/windows/sizes-general.md#dv2-series) szybkich procesorów, pamięci masowej SSD i podwójne współczynnik pamięci core w porównaniu do **standardowe** warstwy. W tym artykule, możesz dowiedzieć się, jak utworzyć aplikację w **PremiumV2** warstwę lub skalować aplikację na **PremiumV2** warstwy.

## <a name="prerequisites"></a>Wymagania wstępne

Do skalowania w górę aplikacji sieci web w taki sposób, aby **PremiumV2**, musisz mieć aplikację sieci Web w usłudze Azure App Service, uruchomionego na niższy niż poziom cenowy **PremiumV2**.

<a name="availability"></a>

## <a name="premiumv2-availability"></a>Dostępność PremiumV2

Warstwa PremiumV2 jest obecnie dostępne dla usługi aplikacji na _Windows_ tylko. Kontenery systemu Linux nie są jeszcze obsługiwane.

PremiumV2 jest już dostępne w regionach najbardziej platformy Azure i rosnącym. Aby sprawdzić, czy jest dostępna w danym regionie, uruchom następujące polecenie z wiersza polecenia platformy Azure [powłoki chmury Azure](../cloud-shell/overview.md):

```azurecli-interactive
az appservice list-locations --sku P1V2
```

Jeśli wystąpi błąd podczas tworzenia aplikacji lub tworzenie planu usługi aplikacji, następnie **PremiumV2** najprawdopodobniej nie jest dostępny w Twoim regionie wybór.

<a name="create"></a>

## <a name="create-an-app-in-premiumv2-tier"></a>Tworzenie aplikacji w warstwie PremiumV2

Warstwy cenowej aplikację usługi aplikacji jest zdefiniowany w [planu usługi aplikacji](azure-web-sites-web-hosting-plans-in-depth-overview.md) uruchamianego na. Można utworzyć planu usługi App Service, samodzielnie lub w ramach tworzenia aplikacji sieci Web.

Podczas konfigurowania planu usługi aplikacji w <a href="https://portal.azure.com" target="_blank">portalu Azure</a>, wybierz pozycję **warstwa cenowa**. 

Wybierz jedną z **PremiumV2** opcje i kliknij przycisk **wybierz**.

![](media/app-service-configure-premium-tier/pick-premium-tier.png)

> [!IMPORTANT] 
> Jeśli nie widzisz **P1V2**, **P2V2**, i **P3V2** jako opcje albo **PremiumV2** nie jest dostępny w Twoim regionie wyboru, lub Konfigurowanie planu usługi aplikacji systemu Linux, które nie obsługuje **PremiumV2**.

## <a name="scale-up-an-existing-app-to-premiumv2-tier"></a>Skalowanie w górę istniejącą aplikację do warstwy PremiumV2

Przed istniejącą aplikację w celu skalowania **PremiumV2** warstwy, upewnij się, że **PremiumV2** jest dostępny w Twoim regionie. Aby uzyskać informacje, zobacz [dostępności PremiumV2](#availability). Jeśli nie jest dostępna w danym regionie, zobacz [skalowanie w górę od nieobsługiwany region](#unsupported).

W zależności od środowiska hostingu skalowaniu mogą wymagać wykonania dodatkowych czynności. 

W <a href="https://portal.azure.com" target="_blank">portalu Azure</a>, otwórz stronę aplikację usługi aplikacji.

W lewym obszarze nawigacji strony aplikacji usługi aplikacji, wybierz **skalowanie w górę (plan usługi App Service)**.

![](media/app-service-configure-premium-tier/scale-up-tier-portal.png)

Wybierz jedną z **PremiumV2** rozmiary, następnie kliknij przycisk **wybierz**.

![](media/app-service-configure-premium-tier/scale-up-tier-select.png)

Jeśli operacja zakończy się pomyślnie, strony Przegląd aplikacji wskazuje, że jest teraz w **PremiumV2** warstwy.

![](media/app-service-configure-premium-tier/finished.png)

### <a name="if-you-get-an-error"></a>Jeśli wystąpi błąd

Niektóre plany usługi aplikacji — nie można skalować do warstwy PremiumV2. Operacja skalowania w górę zwraca błąd, należy najpierw nowy plan usługi aplikacji — dla aplikacji.

Utwórz _Windows_ planu usługi aplikacji w tej samej grupie regionu i zasobu jako istniejącą aplikację usługi aplikacji. Wykonaj kroki opisane w temacie [tworzenie aplikacji w warstwie PremiumV2](#create) ustawić ją na **PremiumV2** warstwy. W razie potrzeby można użyć tej samej konfiguracji skalowania w poziomie jako istniejący plan usługi aplikacji (liczba wystąpień, skalowania automatycznego i tak dalej).

Ponownie otwórz stronę aplikację usługi aplikacji. W lewym panelu nawigacyjnym usługi aplikacji, wybierz **planu usługi aplikacji zmiany**.

![](media/app-service-configure-premium-tier/change-plan.png)

Wybierz utworzony plan usługi aplikacji.

![](media/app-service-configure-premium-tier/select-plan.png)

Po zakończeniu operacji zmiany aplikacja działa **PremiumV2** warstwy.

<a name="unsupported"></a>

## <a name="scale-up-from-an-unsupported-region"></a>Skalowanie w górę od nieobsługiwany region

Jeśli aplikacja będzie działać w regionie, w którym **PremiumV2** jest jeszcze niedostępne, można przenieść aplikację w innym regionie, aby móc korzystać z **PremiumV2**. Dostępne są dwie opcje:

- Utwórz nową aplikację w **PremiumV2** planowanie, a następnie ponownie wdrożyć kod aplikacji. Wykonaj kroki opisane w temacie [tworzenie aplikacji w warstwie PremiumV2](#create) ustawić ją na **PremiumV2** warstwy. W razie potrzeby można użyć tej samej konfiguracji skalowania w poziomie jako istniejący plan usługi aplikacji (liczba wystąpień, skalowania automatycznego i tak dalej).
- Jeśli aplikacja będzie już działać w istniejącym **Premium** warstwy, a następnie aplikacji może klonować z ustawienia aplikacji, parametry połączenia i konfiguracji wdrożenia.

    ![](media/app-service-configure-premium-tier/clone-app.png)

    W **aplikacji w klonowania** stronę, można utworzyć nowy plan usługi aplikacji w regionie i określ ustawienia, które chcesz sklonować.

## <a name="automate-with-scripts"></a>Zautomatyzować za pomocą skryptów

Można zautomatyzować tworzenie aplikacji w **PremiumV2** warstwy za pomocą skryptów przy użyciu [interfejsu wiersza polecenia Azure](/cli/azure/install-azure-cli) lub [programu Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure

Poniższe polecenie tworzy plan usługi aplikacji w _P1V2_. Można go uruchomić w powłoce chmury. Opcje `--sku` są P1V2, _P2V2_, i _P3V2_.

```azurecli-interactive
az appservice plan create \
    --resource-group <resource_group_name> \
    --name <app_service_plan_name> \
    --sku P1V2
```

### <a name="azure-powershell"></a>Azure PowerShell

Poniższe polecenie tworzy plan usługi aplikacji w _P1V2_. Opcje `-WorkerSize` są _małych_, _średni_, i _duży_.

```PowerShell
New-AzureRmAppServicePlan -ResourceGroupName <resource_group_name> `
    -Name <app_service_plan_name> `
    -Location <region_name> `
    -Tier "PremiumV2" `
    -WorkerSize "Small"
```
## <a name="more-resources"></a>Więcej zasobów

[Skalowanie w górę aplikacji na platformie Azure](web-sites-scale.md)  
[Ręczne lub automatyczne skalowanie liczby wystąpień](../monitoring-and-diagnostics/insights-how-to-scale.md)