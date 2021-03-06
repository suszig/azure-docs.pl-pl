---
title: "Konfigurowanie środowiska źródłowego (serwerów fizycznych do platformy Azure) | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób konfigurowania środowiska lokalnego do rozpoczęcia replikacji serwery fizyczne z systemem Windows lub Linux na platformie Azure."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: anoopkv
ms.openlocfilehash: 96004a70547c4bfb3a1a3bfadecb1304e4910b52
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="set-up-the-source-environment-physical-server-to-azure"></a>Konfigurowanie środowiska źródłowego (serwera fizycznego na platformie Azure)

W tym artykule opisano sposób konfigurowania środowiska lokalnego do rozpoczęcia replikacji serwery fizyczne z systemem Windows lub Linux na platformie Azure.

## <a name="prerequisites"></a>Wymagania wstępne

Artykuł założono, że masz:
1. Magazyn usług odzyskiwania i w [portalu Azure](http://portal.azure.com "portalu Azure").
3. Komputer fizyczny, na którym jest instalowany serwer konfiguracji.

### <a name="configuration-server-minimum-requirements"></a>Minimalne wymagania dotyczące konfiguracji serwera
W poniższej tabeli wymieniono minimalne wymagania dotyczące sprzętu, oprogramowania i wymagania sieciowe dotyczące serwera konfiguracji.
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> Serwery proxy oparty na protokole HTTPS nie są obsługiwane przez serwer konfiguracji.

## <a name="choose-your-protection-goals"></a>Wybranie celów ochrony

1. W portalu Azure, przejdź do **usług odzyskiwania** magazyny bloku, a następnie wybierz magazyn.
2. W **zasobów** menu magazynu, kliknij przycisk **wprowadzenie** > **usługi Site Recovery** > **krok 1: Przygotowanie infrastruktury** > **cel ochrony**.

    ![Wybieranie celów](./media/physical-azure-set-up-source/choose-goals.png)
3. W **cel ochrony**, wybierz pozycję **do platformy Azure** i **nie zwirtualizowanych/inne**, a następnie kliknij przycisk **OK**.

    ![Wybieranie celów](./media/physical-azure-set-up-source/physical-protection-goal.png)

## <a name="set-up-the-source-environment"></a>Konfigurowanie środowiska źródłowego

1. W **Przygotuj źródło**, jeśli nie masz serwera konfiguracji, kliknij przycisk **+ serwer konfiguracji** dodanie.

  ![Konfiguracja źródła](./media/physical-azure-set-up-source/plus-config-srv.png)
2. W **Dodaj serwer** bloku, sprawdź, czy **serwera konfiguracji** pojawia się w **typ serwera**.
4. Pobierz plik instalacyjny instalacja Unified usługi Site Recovery.
5. Pobierz klucz rejestracji magazynu. Po uruchomieniu Instalatora Unified muszą się klucz rejestracji. Klucz jest ważny przez pięć dni po jego wygenerowaniu.

    ![Konfiguracja źródła](./media/physical-azure-set-up-source/set-source2.png)
6. Na komputerze, używając jako serwer konfiguracji, uruchom **Unified instalacja usługi Azure Site Recovery** do zainstalowania serwera konfiguracji, serwer przetwarzania i główny serwer docelowy.

#### <a name="run-azure-site-recovery-unified-setup"></a>Uruchom usługi Azure Site Recovery Unified Instalatora

> [!TIP]
> Rejestracja serwera konfiguracji kończy się niepowodzeniem, jeśli czas na zegara systemowego jest więcej niż pięć minut wylogowuje na czas lokalny. Synchronizowanie zegara systemowego z [serwer czasu](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) przed rozpoczęciem instalacji.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Serwer konfiguracji można zainstalować za pomocą wiersza polecenia. Aby uzyskać więcej informacji, zobacz [Instalowanie serwera konfiguracji za pomocą narzędzia wiersza polecenia](http://aka.ms/installconfigsrv).


## <a name="common-issues"></a>Typowe problemy

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Kolejne kroki

Następny krok polega na [Konfigurowanie środowiska docelowego](physical-azure-set-up-target.md) na platformie Azure.
