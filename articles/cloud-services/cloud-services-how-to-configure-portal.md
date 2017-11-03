---
title: "Jak skonfigurować usługi w chmurze (portal) | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować usługi w chmurze na platformie Azure. Dowiedz się zaktualizować konfigurację usługi w chmurze i konfigurowania dostępu zdalnego do wystąpień roli. Te przykłady, użyj portalu Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 7308f3c0-825e-499d-bfa5-c60f86371921
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: adegeo
ms.openlocfilehash: a7e891d05ffe4cc2b4f68dce072a81499cc6de80
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-cloud-services"></a>Jak skonfigurować usługi w chmurze
> [!div class="op_single_selector"]
> * [Witryna Azure Portal](cloud-services-how-to-configure-portal.md)
> * [Klasyczna witryna Azure Portal](cloud-services-how-to-configure.md)
>
>

Najczęściej używane ustawienia dla usługi w chmurze można skonfigurować w portalu Azure. Jeśli wolisz bezpośrednio aktualizować pliki konfiguracji, pobierz odpowiedni plik, a następnie przekaż jego zaktualizowaną wersję i zaktualizuj usługę w chmurze, wprowadzając zmiany w konfiguracji. W obu przypadkach aktualizacje konfiguracji są przekazywane do wszystkich wystąpień ról.

Można również zarządzać wystąpień role usługi w chmurze lub pulpit zdalny do nich.

Azure tylko sposobem zapewnienia dostępności usługi 99,95% podczas aktualizowania konfiguracji, jeśli masz co najmniej dwóch wystąpień roli dla każdej roli. Umożliwiająca jednej maszyny wirtualnej do przetworzenia żądania klientów podczas innych aktualizacji. Aby uzyskać więcej informacji, zobacz [umowy dotyczące poziomu usług](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Zmiana usługi w chmurze
Po otwarciu [portalu Azure](https://portal.azure.com/), przejdź do usługi w chmurze. W tym miejscu możesz zarządzać wiele aspektów.

![Ustawienia strony](./media/cloud-services-how-to-configure-portal/cloud-service.png)

**Ustawienia** lub **wszystkie ustawienia** spowoduje to otwarcie łącza **ustawienia** bloku, w którym można zmienić **właściwości**, zmień **konfiguracji**, zarządzanie **certyfikaty**, Instalator **reguły alertów**i zarządzanie nimi **użytkowników** mających dostęp do tej usługi w chmurze.

![Blok ustawień usługi w chmurze Azure](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

### <a name="manage-guest-os-version"></a>Zarządzanie wersji systemu operacyjnego gościa

Domyślnie program Azure okresowo aktualizuje użytkownika gościa systemu operacyjnego do najnowszych obsługiwanych obrazu w ramach rodziny systemów operacyjnych, określoną w konfiguracji usługi (cscfg), takie jak Windows Server 2016.

Jeśli potrzebujesz pod kątem określonych wersji systemu operacyjnego, można ustawić **konfiguracji** bloku.

![Ustaw wersję systemu operacyjnego](./media/cloud-services-how-to-configure-portal/cs-settings-config-guestosversion.png)


>[!IMPORTANT]
> Wybranie określonych wersji systemu operacyjnego spowoduje wyłączenie automatycznego systemu operacyjnego aktualizacji i umożliwia stosowanie poprawek programu odpowiedzialności. Należy upewnić się, że wystąpienia roli otrzymują aktualizacje lub może narazić luk w zabezpieczeniach przez aplikację.

## <a name="monitoring"></a>Monitorowanie
Alerty można dodać do usługi w chmurze. Kliknij przycisk **ustawienia** > **reguły alertu** > **Dodaj alert**.

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

W tym miejscu możesz skonfigurować alert. Z **Metryka** listy rozwijanej, możesz skonfigurować alert dla następujących typów danych.

* Odczytu dysku
* Zapisu na dysku
* Sieci w
* Sieci limit
* Procent użycia procesora CPU

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Skonfiguruj funkcję monitorowania z metryki kafelka
Zamiast **ustawienia** > **reguły alertów**, możesz kliknąć jeden z metryki kafelków w **monitorowanie** sekcji **usługi w chmurze** bloku.

![Monitorowanie usługi w chmurze](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

W tym miejscu można dostosować wykres używane z kafelka lub dodać reguły alertów.

## <a name="reboot-reimage-or-remote-desktop"></a>Ponowne uruchomienie komputera, odtworzenia z obrazu lub pulpitu zdalnego
W tej chwili nie można skonfigurować przy użyciu pulpitu zdalnego **portalu Azure**. Jednak można skonfigurować go za pośrednictwem [klasycznego portalu Azure](cloud-services-role-enable-remote-desktop.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), lub za pomocą [programu Visual Studio](../vs-azure-tools-remote-desktop-roles.md).

Najpierw kliknij w wystąpieniu usługi chmury.

![Wystąpienie usługi w chmurze](./media/cloud-services-how-to-configure-portal/cs-instance.png)

W bloku czy otwiera można zainicjować Podłączanie pulpitu zdalnego, zdalnie ponownie wystąpienie lub zdalnie odtworzyć (uruchomienie z obrazem świeże) wystąpienia.

![Przyciski wystąpienia usługi chmury](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

## <a name="reconfigure-your-cscfg"></a>Skonfiguruj ponownie Twoje .cscfg
Może zaistnieć konieczność zmiany konfiguracji usługi w chmurze za pośrednictwem [konfiguracji usługi (cscfg)](cloud-services-model-and-package.md#cscfg) pliku. Najpierw należy pobrać plik .cscfg, zmodyfikuj go, a następnie przekaż go.

1. Polecenie **ustawienia** ikony lub **wszystkie ustawienia** łącze, aby otworzyć **ustawienia** bloku.

    ![Ustawienia strony](./media/cloud-services-how-to-configure-portal/cloud-service.png)
2. Polecenie **konfiguracji** elementu.

    ![Blok konfiguracji](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
3. Polecenie **Pobierz** przycisku.

    ![Do pobrania](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)
4. Po zaktualizowaniu pliku konfiguracji usługi, przekazywanie i stosowania aktualizacji konfiguracji:

    ![Upload](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png)
5. Wybierz plik .cscfg, a następnie kliknij przycisk **OK**.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [wdrażania usługi w chmurze](cloud-services-how-to-create-deploy-portal.md).
* Skonfiguruj [niestandardowej nazwy domeny](cloud-services-custom-domain-name-portal.md).
* [Usługi w chmurze zarządzanie](cloud-services-how-to-manage-portal.md).
* Skonfiguruj [certyfikaty ssl](cloud-services-configure-ssl-certificate-portal.md).
