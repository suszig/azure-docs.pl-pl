---
title: "Konfigurowanie środowiska źródłowego (VMware do platformy Azure) | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób konfigurowania środowiska lokalnego do rozpoczęcia replikacji maszyn wirtualnych VMware do platformy Azure."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/23/2017
ms.author: anoopkv
ms.openlocfilehash: 32a3f7498d3c8891178818436e33221f91ae2f8f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="set-up-the-source-environment-vmware-to-azure"></a>Konfigurowanie środowiska źródłowego (VMware do platformy Azure)
> [!div class="op_single_selector"]
> * [Z programu VMware do platformy Azure](./site-recovery-set-up-vmware-to-azure.md)
> * [Fizycznych do platformy Azure](./site-recovery-set-up-physical-to-azure.md)

W tym artykule opisano sposób konfigurowania środowiska lokalnego do rozpoczęcia replikacji maszyn wirtualnych uruchomionych na VMware do platformy Azure.

## <a name="prerequisites"></a>Wymagania wstępne

Artykule przyjęto założenie, że utworzono już:
- Magazyn usług odzyskiwania, w [portalu Azure](http://portal.azure.com "portalu Azure").
- Dedykowane konto w oprogramowaniu VMware vCenter sieci używanej do [automatyczne odnajdowanie](./site-recovery-vmware-to-azure.md).
- Maszynę wirtualną, na którym jest instalowany serwer konfiguracji.

## <a name="configuration-server-minimum-requirements"></a>Minimalne wymagania dotyczące konfiguracji serwera
W poniższej tabeli wymieniono minimalne wymagania dotyczące sprzętu, oprogramowania i wymagania sieciowe dotyczące serwera konfiguracji.

> [!IMPORTANT]
> Podczas wdrażania serwera konfiguracji ochrony maszyn wirtualnych VMware, zaleca się wdrożyć go jako **wysokiej dostępności (HA)** maszyny wirtualnej.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> Serwery proxy oparty na protokole HTTPS nie są obsługiwane przez serwer konfiguracji.

## <a name="choose-your-protection-goals"></a>Wybranie celów ochrony

1. W portalu Azure, przejdź do **usług odzyskiwania** magazyn bloku i wybierz magazyn.
2. W menu zasobów magazynu, przejdź do **wprowadzenie** > **usługi Site Recovery** > **krok 1: Przygotowanie infrastruktury** > **cel ochrony**.

    ![Wybieranie celów](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. W **cel ochrony**, wybierz pozycję **do platformy Azure**i wybierz polecenie **tak, z programem VMware vSphere Hypervisor**. Następnie kliknij przycisk **OK**.

    ![Wybieranie celów](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-source-environment"></a>Konfigurowanie środowiska źródłowego
Konfigurowanie środowiska źródłowego obejmuje dwa działania głównego:

- Zainstaluj i Zarejestruj serwer konfiguracji z usługą Site Recovery.
- Odnajdywanie maszyn wirtualnych lokalnie przez nawiązanie połączenia lokalnego VMware vCenter lub vSphere EXSi hosty usługi Site Recovery.

### <a name="step-1-install-and-register-a-configuration-server"></a>Krok 1: Zainstaluj i Zarejestruj serwer konfiguracji

1. Kliknij przycisk **krok 1: Przygotowanie infrastruktury** > **źródła**. W **Przygotuj źródło**, jeśli nie masz serwera konfiguracji, kliknij przycisk **+ serwer konfiguracji** dodanie.

    ![Konfiguracja źródła](./media/site-recovery-set-up-vmware-to-azure/set-source1.png)
2. Na **Dodaj serwer** bloku, sprawdź, czy **serwera konfiguracji** pojawia się w **typ serwera**.
4. Pobierz plik instalacyjny instalacja Unified usługi Site Recovery.
5. Pobierz klucz rejestracji magazynu. Po uruchomieniu Instalatora Unified muszą się klucz rejestracji. Klucz jest ważny przez pięć dni po jego wygenerowaniu.

    ![Konfiguracja źródła](./media/site-recovery-set-up-vmware-to-azure/set-source2.png)
6. Na komputerze, używając jako serwer konfiguracji, uruchom **Unified instalacja usługi Azure Site Recovery** do zainstalowania serwera konfiguracji, serwer przetwarzania i główny serwer docelowy.

#### <a name="run-azure-site-recovery-unified-setup"></a>Uruchom usługi Azure Site Recovery Unified Instalatora

> [!TIP]
> Rejestracja serwera konfiguracji kończy się niepowodzeniem, jeśli czas na zegara systemowego różni się od czasu lokalnego przez ponad pięć minut. Synchronizowanie zegara systemowego z [serwer czasu](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) przed rozpoczęciem instalacji.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Serwer konfiguracji można zainstalować za pomocą wiersza polecenia. Aby uzyskać więcej informacji, zobacz [instalowany serwer konfiguracji przy użyciu narzędzia wiersza polecenia](http://aka.ms/installconfigsrv).

#### <a name="add-the-vmware-account-for-automatic-discovery"></a>Dodaj konto VMware automatycznego wykrywania

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="step-2-add-a-vcenter"></a>Krok 2: Dodaj vCenter
Aby umożliwić usługi Azure Site Recovery odnaleźć maszyn wirtualnych działających w środowisku lokalnym, należy połączyć z usługą Site Recovery hostach ESXi vSphere lub programu VMware vCenter Server, na których.

Wybierz **+ vCenter** można uruchomić połączenia programu VMware vCenter server lub VMware vSphere ESXi hosta.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Typowe problemy
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Kolejne kroki
[Konfigurowanie środowiska docelowego](./site-recovery-prepare-target-vmware-to-azure.md) na platformie Azure.
